After providing the ML package with a name, you should see your new ML package in your project dashboard. With our package created, let's investigate creating a pipeline that can train our newly deployed ML package.

# Creating pipelines

An ML pipeline is a series of steps taken to produce an ML model. During the pipeline process, we will provide an input, and once completed, the pipeline will produce a set of useable outputs and logs that lead to the creation of an ML skill.

In AI Center, there are three types of pipelines, as outlined here:

+   - **Training pipeline**: Produces a newly trained **ML Package** version from an input ML package and dataset
+   - **Evaluation pipeline**: Produces a set of metrics and logs from a newly trained **ML Package** version and dataset
+   - **Full pipeline**: A full data process pipeline, performing both training and evaluation pipelines

For this use case, let's use our newly deployed training and evaluation datasets to run a training and evaluation pipeline. Proceed as follows:

+   1. Navigate to **Pipelines** by clicking the **Pipelines** tab in the project dashboard.
+   2. Click **Create New** to create a new pipeline, selecting the following:
+      - **Pipeline type**: `Train run`
+      - **Choose package**: `TextClassification`
+      - **Choose package major version**: `6`
+      - **Choose package minor version**: `0`
+      - **Choose input dataset**: `Email Classification/train/`
+      - **Enable GPU**: Turn this off
+      Under **Environment Variable**, add the following environment variables:
+      - `input_column`: `text`
+      - `target_column`: `label`