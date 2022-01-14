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
