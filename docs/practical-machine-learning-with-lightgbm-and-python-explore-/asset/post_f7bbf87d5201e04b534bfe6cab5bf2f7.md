
![](img/31b70e4a77c3afbe11032f68ab874b02_192_0.png)

Figure 9.2 – SageMaker ML pipeline for Census Income classification

Our approach is to create the entire pipeline using a Jupyter Notebook running in SageMaker Studio. The sections that follow explain and go through the code for each pipeline step, starting with setting up the SageMaker session.

## Setting up a SageMaker session

The following steps assume you have already created an AWS account and set up a SageMaker domain to get started. If not, the following documentation can be referenced to do so:

- Prerequisites: https://docs.aws.amazon.com/sagemaker/latest/dg/gs-set-up.html
- Onboarding to a SageMaker domain: https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html

We must initialize the SageMaker session and create S3, SageMaker, and SageMaker Runtime clients via boto3 to get started:

```python
sess = sagemaker.Session()
region = sess.boto_region_name
s3_client = boto3.client("s3", region_name=region)
sm_client = boto3.client("sagemaker", region_name=region)
sm_runtime_client = boto3.client("sagemaker-runtime")
```

We will use Amazon S3 to store our training data, source code, and all data and artifacts created by the pipeline, such as the serialized model. Our data and artifacts are split into a read bucket and a separate write bucket. This is a standard best practice as it separates the concerns for data storage.

SageMaker sessions have the concept of a default S3 bucket for the session. If no default bucket name is supplied, one is generated, and the bucket is created for you. Here, we're grabbing a reference to the bucket. This is our output or write bucket. The read bucket is a bucket we've created previously that stores our training data:

```python
write_bucket = sess.default_bucket()
write_prefix = "census-income-pipeline"
read_bucket = "sagemaker-data"
read_prefix = "census-income"
```

The source code, configuration, and output of each of the steps in the pipeline are captured in folders within our S3 write bucket. It's useful to create variables for each S3 URI to avoid errors when repeatedly referring to data, like so:

```python
input_data_key = f"s3://{read_bucket}/{read_prefix}"
census_income_data_uri = f"{input_data_key}/census-income.csv"
output_data_uri = f"s3://{write_bucket}/{write_prefix}/"
scripts_uri = f"s3://{write_bucket}/{write_prefix}/scripts"
```

SageMaker needs us to specify the compute instance types we want to use when running the jobs for training, processing, Clarify, and prediction. In our example, we're using m5.large instances. Most EC2 instance types can be used with SageMaker. However, a few special instance types that support GPUs and deep learning frameworks are also available:

```python
train_model_id, train_model_version, train_scope = "lightgbm-classification-model", "*", "training"
process_instance_type = "ml.m5.large"
train_instance_count = 1
train_instance_type = "ml.m5.large"
predictor_instance_count = 1
predictor_instance_type = "ml.m5.large"
clarify_instance_count = 1
clarify_instance_type = "ml.m5.large"
```

SageMaker uses standard EC2 instances for training but runs specific Docker images on the instances to provide ML functionality. Amazon SageMaker provides many prebuilt Docker images for various ML frameworks and stacks.

The SageMaker SDK also provides a function to search for images that are compatible with the instance type we need within the AWS region we are using. We can search for an image using retrieve:

```python
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

```python
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

## Preprocessing step

Setting up our preprocessing step has two parts: creating a Python script that performs the preprocessing and creating a processor that is added to the pipeline.

The script we'll be using is a regular Python script with a main function. We'll use scikit-learn to do our preprocessing. The preprocessing script hasn't been entirely reproduced here but is available in our source code repository. Notably, when the pipeline executes the step, the data is retrieved from S3 and added to a local staging directory on the preprocessing instance. From here, we can read the data using standard pandas tooling:

```python
local_dir = "/opt/ml/processing"
input_data_path = os.path.join("/opt/ml/processing/census-income",
                               "census-income.csv")

logger.info("Reading claims data from {} ".format(input_data_path))
df = pd.read_csv(input_data_path)
```

Similarly, after processing is complete, we can write the results to a local directory, from which SageMaker retrieves it and uploads it to S3:

```python
train_output_path = os.path.join(f"{local_dir}/train", "train.csv")
X_train.to_csv(train_output_path, index=False)
```

With a preprocessing script defined, we need to upload it to S3 for the pipeline to be able to use it:

```python
s3_client.upload_file(
    Filename="src/preprocessing.py", Bucket=write_bucket,
    Key=f"{write_prefix}/scripts/preprocessing.py"
)
```

We can define the preprocessing step as follows. First, we must create an SKLearnProcessor instance:

```python
sklearn_processor = SKLearnProcessor(
    framework_version="0.23-1",
    role=sagemaker_role,
    instance_count=1,
    instance_type=process_instance_type,
    base_job_name=f"{base_job_name_prefix}-processing",
)
```

SKLearnProcessor handles the processing task for jobs that require scikit-learn. We specify the scikit-learn framework version and the instance type and count we defined earlier.

The processor is then added to ProcessingStep for use in the pipeline:

```python
process_step = ProcessingStep(
    name="DataProcessing",
    processor=sklearn_processor,
    inputs=[...],
    outputs=[...],
)
```

```python
job_arguments=[
    "--train-ratio", "0.8",
    "--validation-ratio", "0.1",
    "--test-ratio", "0.1"
],
code=f"s3://{write_bucket}/{write_prefix}/scripts/preprocessing.py"
)
```

inputs and outputs are defined using the ProcessingInput and ProcessingOutput wrappers, as shown here:

```python
inputs = [ ProcessingInput(source=bank_marketing_data_uri,
               destination="/opt/ml/processing/bank-marketing") ]
outputs = [ ProcessingOutput(destination=f"{processing_output_uri}/train_data", output_name="train_data",
               source="/opt/ml/processing/train"), ... ]
```

ProcessingStep takes our scikit-learn processor and the inputs and outputs for the data. The ProcessingInput instances define the S3 source and local directory destination to facilitate copying the data (these are the same local directories our preprocessing script uses). Similarly, the ProcessingOutput instances take the local directory source and S3 destinations. We also set job arguments, which are passed to the preprocessing script as CLI arguments.

Having set up the preprocessing step, we can move on to training.

## Model training and tuning

We define a training script in the same way as a preprocessing script: a Python script with a main function that uses our standard Python tools, such as scikit-learn, to train a LightGBM model. However, we also need to install the LightGBM library itself.

An alternative to installing the library is building it into a Docker image and using it as our training image in SageMaker. This is the canonical way of managing environments in SageMaker. However, it entails significant work and includes the long-term need to maintain the image over time. Alternatively, if we only need to install a handful of dependencies, we can do that directly from our training script, as shown here.

We must define a helper function to install packages and then use it to install LightGBM:

```python
def install(package):
    subprocess.check_call([sys.executable, "-q", "-m", "pip",
    "install", package])
install("lightgbm")
import lightgbm as lgb
```
