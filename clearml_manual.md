# Fundamentals
- **Task** is your Python code, e.g., data processing, model training, etc.
    By default a task's type is set to *training*.
- **Queue** is a queue of your tasks
- **Agent (worker)** is the machine in which your tasks will be executed. One queue could be served by one or multiple agents.

# ClearML components
- **ClearML server** provides a user interface to track your experiments.
    The server is open-sourced and there are [many different ways](https://clear.ml/docs/latest/docs/deploying_clearml/clearml_server#deployment) to deploy it.
- **clearml-agent**  which pulls tasks from queue and execute them
