# MLOps
- [SageMarker](sagemaker.md)

# Quantum computing
## Explanation
- [Trapping ions](https://www.youtube.com/watch?v=j1SKprQIkyE)

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
