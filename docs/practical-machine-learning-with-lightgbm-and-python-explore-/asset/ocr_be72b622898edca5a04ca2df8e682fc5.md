SageMaker uses standard EC2 instances for training but runs specific Docker images on the instances to provide ML functionality. Amazon SageMaker provides many prebuilt Docker images for various ML frameworks and stacks.

The SageMaker SDK also provides a function to search for images that are compatible with the instance type we need within the AWS region we are using. We can search for an image using retrieve:

```
train_image_uri = retrieve(
    region="us-east-1",
    framework=None,
    model_id=train_model_id,
    model_version=train_model_version,
    image_scope=train_scope,
    instance_type=train_instance_type
)
```

We specify None for the framework parameter as we manage the LightGBM installation ourselves.

To parameterize our pipeline, we must define SageMaker workflow parameters from the sagemaker.workflow.parameters package. Wrappers are available for various parameter types:

```
train_instance_type_param = ParameterString(
    name="TrainingInstanceType",
    default_value=train_instance_type)

train_instance_count_param = ParameterInteger(
    name="TrainingInstanceCount",
    default_value=train_instance_count)

deploy_instance_type_param = ParameterString(
    name="DeployInstanceType",
    default_value=predictor_instance_type)

deploy_instance_count_param = ParameterInteger(
    name="DeployInstanceCount",
    default_value=predictor_instance_count)
```

With our pipeline parameters, S3 data paths, and other configuration variables set, we can move on to creating our pipeline's preprocessing step.

# Preprocessing step

Setting up our preprocessing step has two parts: creating a Python script that performs the preprocessing and creating a processor that is added to the pipeline.