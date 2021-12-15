# MLOps
- [SageMarker](sagemaker.md)

# Quantum computing
## Explanation
- [Trapping ions](https://www.youtube.com/watch?v=j1SKprQIkyE)
- [Different technologies to build quantum computers](https://www.youtube.com/watch?v=OGsu5MIzruw)

# Modelling tools
## Uncertainty quantification
- [UQ360](https://uq360.mybluemix.net/resources/guidance): provides confidence intervals for your ML models.
    It also provides model calibration functionalities. If you have already trained your models, you could make use
    of the [meta model](https://github.com/IBM/UQ360/tree/main/examples/blackbox_metamodel).

# Tools
## AWS Lambda
Use case: deploy ML models to run inference on new data

### Setup
- Generate your [credentials](https://console.aws.amazon.com/iam/home?#/security_credentials)
- Configure your credentials [with the CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)
### Tutorials
- [Hello world](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-images.html#gettingstarted-images-prereq)
- [Deploy Python image](https://docs.aws.amazon.com/lambda/latest/dg/python-image.html)
- [Schedule your Lambda function](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/RunLambdaSchedule.html)
- [Clean up](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-images.html#gettingstarted-image-cleanup)

# PyTorch
- Reproducibility
```
import os
import torch
import random

import numpy as np
os.environ['CUBLAS_WORKSPACE_CONFIG'] = ':4096:8'
torch.manual_seed(0)
random.seed(0)
np.random.seed(0)
g = torch.Generator()
g.manual_seed(0)
torch.use_deterministic_algorithms(True)
```

# Pytorch Lightning
## grid.ai
A platform to leverage Pytorch Lightning and other ML frameworks without managing infrastructure by yourself
- [CLI workflow](https://docs.grid.ai/start-here/typical-workflow-cli-user)
- [Kaggle competition best practices](https://devblog.pytorchlightning.ai/best-practices-to-rank-on-kaggle-competition-with-pytorch-lightning-and-grid-ai-spot-instances-54aa5248aa8e)

# Tips
- Execute a bash in a new container of a docker image `docker run --rm -it --entrypoint bash <image-name-or-id>
` (https://stackoverflow.com/a/43309168)

# References
- https://mlops.neptune.ai/

# Library
## Misc
- [Neural networks sparsification](https://github.com/neuralmagic/sparseml)

## Active Learning
- https://github.com/decile-team/distil/
