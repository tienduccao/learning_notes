# How to
- [Setup studio, create experiments](https://aws.amazon.com/getting-started/hands-on/build-train-deploy-monitor-machine-learning-model-sagemaker-studio/?)

## Experiment
- [Experiments SDK](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-experiments/tensorflow2-california-housing-regression-experiment/tensorflow2-california-housing-regression-experiment.ipynb)

## Debugger
- [Tensor analysis](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_analysis). 
Note: need to modify all constructors of Rule's subclasses (e.g., "GradientsLayer", "ActivationInputs") from `super().__init__(base_trial)` to
`super().__init__(base_trial, None)`.

## Tuning
- [Hyperparameters tuning](https://github.com/aws/amazon-sagemaker-examples/blob/master/hyperparameter_tuning/xgboost_direct_marketing/hpo_xgboost_direct_marketing_sagemaker_python_sdk.ipynb). Adjust `max_jobs` and `max_parallel_jobs` of `HyperparameterTuner` to speed up your tuning process.

## Pipeline
- [Build a pipeline](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-pipelines/tabular/abalone_build_train_deploy/sagemaker-pipelines-preprocess-train-evaluate-batch-transform.html#)
- [Visualize pipeline](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines-studio-list-pipelines.html)
- https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html

## Clarify
- [Standalone lib](https://github.com/aws/amazon-sagemaker-clarify)
- [Bias metrics definitions](https://pages.awscloud.com/rs/112-TZM-766/images/Fairness.Measures.for.Machine.Learning.in.Finance.pdf)
- [Usage guide](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-fairness-and-explainability.html)
- [Pre-training bias metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-data-bias.html)
- [Post-training bias metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-post-training-bias.html)

# References
- [Notebook available EC2 instances](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks-available-instance-types.html)
- [ResourceLimitExceeded](https://aws.amazon.com/premiumsupport/knowledge-center/resourcelimitexceeded-sagemaker/)
- [Service quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html#limits_sagemaker)
- [Pricing](https://aws.amazon.com/sagemaker/pricing/)
- [SageMaker examples](https://sagemaker-examples.readthedocs.io/en/latest/index.html)

