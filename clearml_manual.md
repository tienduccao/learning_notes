# Fundamentals
- **Task** is your Python code, e.g., data processing, model training, etc.
    By default a task's type is set to *training*.
- **Queue** is a queue of your tasks
- **Agent (worker)** is the machine in which your tasks will be executed. One queue could be served by one or multiple agents. Agents could be executed under [services mode](https://clear.ml/docs/latest/docs/clearml_agent/#services-mode).

# ClearML components
- **ClearML server** provides a user interface to track your experiments.
    The server is open-sourced and there are [many different ways](https://clear.ml/docs/latest/docs/deploying_clearml/clearml_server#deployment) to deploy it.
- **clearml-agent**  which pulls tasks from queue and execute them

# Workflows
## Execute a single task
Firstly you need to configure the access to a ClearML server (this must be done only once). 
As mentioned previously, you could deploy your own server.
Or you could simply register a free account at https://app.clear.ml/.
After installing `clearml` via `pip install clearml`, run the following command `clearml-init` and follow
its instructions.

To execute your task, simply need to add the following 2 lines of code to your Python script
```
from cleaml import Task
task = Task.init(project_name='<your project name>', task_name='<your experiment name>')
```
Your project will be created if it's not available yet.

In order to run your code in a specific queue, you'll need to call `task.execute_remotely(queue_name=<remote_queue_name>)`. Note that you must have an agent listening to your queue.
The detailed instructions to setup your agent could be found [here](https://clear.ml/docs/latest/docs/clearml_agent).

To log necessary data when running your task, you could use
[Logger](https://clear.ml/docs/latest/docs/fundamentals/logger/).

## Pipelines
Reference 
- https://clear.ml/docs/latest/docs/fundamentals/pipelines
- https://clear.ml/docs/latest/docs/references/sdk/automation_controller_pipelinecontroller#add_step

A pipeline consisted of multiple tasks.
A **PipelineController** holds the execution logic of the pipeline and is executed on the **services** queue machine.
A visual plot of the pipeline could be found under Results/Plot tab of the ControllerTask.

Each task of the pipeline must have `task.execute_remotely` in order to register the task
with ClearML server but not to execute it immediately.
Later on we'll execute all these tasks with the pipeline.

- `Pipeline.add_step` provides the `parents` parameter to define the connections between pipelines' steps
- `Pipeline.add_step` provides `pre_execute_callback` and `post_execute_callback` parameters to customize the pipeline logic

## Hyperparameters optimization
https://clear.ml/docs/latest/docs/fundamentals/hpo

Workflow
- Configure an Optimization Task with a base task whose parameters will be optimized, and a set of parameter values to test
- Clone the base task. Each clone's parameter is overridden with a value from the optimization task
- Enqueue each clone for execution by a ClearML Agent
- The Optimization Task records and monitors the cloned tasks' configuration and execution details, and returns a summary of the optimization results in tabular and parallel coordinate formats, and in a scalar plot.

## Scale your tasks with AutoScaler
The AutoScaler pull your tasks from one or severals queues and distribute them to available computing 
instances (e.g., AWS EC2). In each instance
- Required packages will be installed
- Necessary credentials will be configured, e.g., git repository, data access credentials (AWS S3 for example)
- A clearml-agent will be launched to execute your task
- Then the agent continues to look for new tasks from your queues

When there's no more available tasks, each instance will be terminated after a maximum threshold of idle time has been reached.

You could use the AutoScaler for experimenting different ideas at the same time, or for speeding up your
hyperparameters search.

# Setup
## Deploy your own ClearML server with AWS
### AWS setup
- https://clear.ml/docs/latest/docs/deploying_clearml/clearml_server_aws_ec2_ami#clearml-server-aws-community-amis
- Recommended EC2 instances: t3.large or t3a.large (cheaper)
- Configure EC2 instance's security groups' policies to allow TCP traffic from port 8080 (web server), 8008 (API server) and 8081 (file server)

### Server configuration
- [Configure user authentication](https://clear.ml/docs/latest/docs/deploying_clearml/clearml_server_config#using-hashed-passwords). Remember to [restart the server](https://github.com/allegroai/clearml-server#restarting-clearml-server).
- Now you could login with your credentials.
- TODO add https support with ELB

### Client configuration
- Obtain your app credentials under `http://<your clearml server IP>/profile`
- Configure the credential via `clearml-init` command or by editing `~/clearml.conf` directly
```
api {
    web_server: http://<your clearml server IP>:8080
    api_server: http://<your clearml server IP>:8008
    files_server: http://<your clearml server IP>:8081
    credentials {
        "access_key" = "your access key"
        "secret_key" = "your secret key"
    }
}
```
## Autoscaler with AWS
### Configuration
Create a file named `aws_autoscaler.yaml` like this one.
Each 5 minutes (`polling_interval_time_min`) the Autoscaler will check for tasks from `gpu_queue` queue
and launch at most 3 GPU instances (g3s.xlarge) to execute the available tasks.
Spawned instances will be terminated after 10 idle minutes (`max_idle_time_min`).

```
configurations:
  extra_clearml_conf: ''
  extra_trains_conf: ''
  extra_vm_bash_script: ''
  queues:
    gpu_queue:
    - - aws4gpu
      - 3
  resource_configurations:
    aws4gpu:
      ami_id: ami-04129d3de24f88348
      availability_zone: us-west-2c
      ebs_device_name: /dev/sda1
      ebs_volume_size: 100
      ebs_volume_type: gp2
      instance_type: g3s.xlarge
      is_spot: false
      key_name: gpu
      security_group_ids: null
hyper_params:
  cloud_credentials_key: ''
  cloud_credentials_region: us-west-2
  cloud_credentials_secret: ''
  cloud_provider: ''
  default_docker_image: ''
  git_user: ''
  git_pass: ''
  max_idle_time_min: 10
  max_spin_up_time_min: 30
  polling_interval_time_min: 5
  use_credentials_chain: true
  workers_prefix: dynamic_worker
```

The following values could be modified to suite your own needs:
- `ami_id`: currently ClearML will try to launch agents on Ubuntu machines, so you must choose 
AMIs for Ubuntu systems.
- `availability_zone`, `cloud_credentials_region`: modify this value according to your AWS account
- `ebs_volume_size`: (in GB) modify this value according to your storage requirement
- `instance_type`: choose GPU instance if you need
- `git_user`: your GitLab username if you need to clone your private repositories
- `git_pass`: your [GitLab personal access token](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html)
- `use_credentials_chain: true`: to use your AWS credentials under `~/.aws/` folder,
otherwise you need to fill in `cloud_credentials_key` and `cloud_credentials_secret`
- `key_name`: name of your [EC2 key pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)

### Execute the Autoscaler
After installing git and tmux on your ClearML server, execute the following command.
(Modify the `PYTHON_VERSION` if needed)

```
tmux new -s autoscaler
source venv_autoscaler/bin/activate
export PYTHON_VERSION=3.8
wget https://raw.githubusercontent.com/allegroai/clearml/master/examples/services/aws-autoscaler/aws_autoscaler.py
pip install boto3
git clone https://github.com/tienduccao/clearml
cd clearml
git checkout 28945c9cd156c1a6093fb5668569ee298d65898f
cd ..
pip install -e clearml
python aws_autoscaler.py --run
```
