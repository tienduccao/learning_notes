# Fundamentals
- **Task** is your Python code, e.g., data processing, model training, etc.
    By default a task's type is set to *training*.
- **Queue** is a queue of your tasks
- **Agent (worker)** is the machine in which your tasks will be executed. One queue could be served by one or multiple agents.

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
