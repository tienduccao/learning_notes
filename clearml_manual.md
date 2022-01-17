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
