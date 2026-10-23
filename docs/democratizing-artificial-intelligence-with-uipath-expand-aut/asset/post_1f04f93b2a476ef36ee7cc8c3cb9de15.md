
## Future State Design

![](img/05268897c8ac5d80eb3fead1cc8a822a_273_0.png)

Figure 9.6 – High-level future state design

For this use case, automation will have the responsibility of monitoring the accounting team's shared inbox. The automation can run in the background in an attended or unattended scenario, scanning the shared inbox for new emails. Once an email is received, the automation will retrieve the email, read its contents, and classify the email as spam or not spam based on the contents of the email body. Lastly, based on the classification, the automation will route the email to a corresponding folder where an accounting analyst can review it. The folders that automation will route to are listed here:

- Invoices
- Low Confidence
- Spam
- Retrain-Not Spam
- Retrain-Spam

The reason for the Low Confidence email folder is to have a landing zone for any email that automation has low confidence in classifying. We wouldn't want to have a scenario where an invoice is accidentally placed in the Spam folder, so any email the automation has difficulty with will be routed to the Low Confidence folder. We'll also be adding a retraining component to the use case, where incorrectly classified emails can be moved to one of the Retrain folders to retrain our ML skill.

To summarize, as the future state design of this use case, the project we intend to build can be leveraged in either an attended- or unattended-type scenario. The automation will monitor an email inbox and route incoming emails based on their body contents. Based on the automation's classification, incoming emails will be routed to one of three folders. The end user can then more quickly review invoices, without needing to continually clean the shared inbox with this automation.

## Building a solution with AI Center

As we venture into building a use case, we can split development into the following two sections:

1. Creating an ML skill
2. Building automation workflows

We will start with deploying our ML skill into AI Center, and then create an automation workflow in UiPath Studio. Let's begin with creating an ML skill.

### Creating an ML skill

In this section, we will create and train an ML skill for our use case with AI Center. By deploying an ML package and by loading a dataset to train and evaluate our ML model, we will build an ML skill ready to be tested with our automation workflow.

### Creating an AI Center project

Before we can deploy an ML package or create an ML skill, we must first create a project in AI Center. To do so, follow these steps:

1. Navigate to UiPath Automation Cloud and click on AI Center to get started, as highlighted in the following screenshot:

While **Short Message Service (SMS)** text is a little different than email text, it should still prove out the use case of filtering out spam versus not spam. For this project, we have taken the *SMS Spam Collection* dataset and split the data into *Training, Evaluation, Testing,* and *Production* datasets, which can be found at the following GitHub link:
https://github.com/PacktPublishing/Democratizing-Artificial-Intelligence-with-UiPath/tree/main/Chapter09/Email%20Data.

> **Important Note**
> The dataset has been split into small and large datasets. The large dataset is the full set, while the small dataset is just a subset of the large dataset. Feel free to use either, but note that training with the large dataset will take a lot longer than with the small dataset.

Before uploading our data to AI Center, let's take a quick glance here at the training dataset:

| text | label |
|---|---|
| Go until jurong point, crazy.. Available o | ham |
| Ok lar... Joking wif u oni... | ham |
| Free entry in 2 a wkly comp to win FA Cu | spam |
| U dun say so early hor... U c already the | ham |
| Nah I don't think he goes to usf, he lives | ham |
| FreeMsg Hey there darling it's been 3 we | spam |
| Even my brother is not like to speak wit | ham |
| As per your request 'Melle Melle (Oru M | ham |
| WINNER!! As a valued network custome | spam |

Figure 9.8 – Training dataset

With the dataset, we have two columns, **text** and **label**, which contain the following information:

- `text` is the plain text of the SMS message.
- `label` is the classification of either spam or ham.

Now that we have a little bit of context on our datasets, let's add our training dataset to AI Center by following the next steps:

1. Within the project dashboard, click **Datasets** to navigate to the **Datasets** page.
2. Click **Create New** to create a new dataset, providing a name of `Email Classification` for our dataset.
3. After the dataset is created, click **Upload** to upload the training and evaluation folders from the following GitHub link to AI Center: `https://github.com/PacktPublishing/Democratizing-Artificial-Intelligence-with-UiPath/tree/main/Chapter09/Email%20Data`.

Once uploaded, the Email Classification dataset should look like this:

![](img/05268897c8ac5d80eb3fead1cc8a822a_277_0.png)

Figure 9.9 – Email Classification dataset

With our datasets loaded into AI Center, we can now proceed with deploying an ML package and running our pipelines.

### Creating an ML package

As mentioned in *Chapter 6, Understanding Your Tools*, an ML package in AI Center is a folder containing all the code and associated files necessary to serve an ML model.

Because we're performing a text classification exercise, we're going to use UiPath's out-of-the-box EnglishTextClassification ML package. Here are the steps you need to follow:

1. Navigate to **ML Packages** by clicking the **ML Packages** tab in the project dashboard.
2. When presented with **Create a new package**, click **Out of the box Packages**.
3. Underneath **Out of the box Packages**, choose **Language Analysis > EnglishTextClassification**, choosing **Package Version 6.0**.

After providing the ML package with a name, you should see your new ML package in your project dashboard. With our package created, let's investigate creating a pipeline that can train our newly deployed ML package.

### Creating pipelines

An ML pipeline is a series of steps taken to produce an ML model. During the pipeline process, we will provide an input, and once completed, the pipeline will produce a set of useable outputs and logs that lead to the creation of an ML skill.

In AI Center, there are three types of pipelines, as outlined here:

- **Training pipeline**: Produces a newly trained **ML Package** version from an input ML package and dataset
- **Evaluation pipeline**: Produces a set of metrics and logs from a newly trained **ML Package** version and dataset
- **Full pipeline**: A full data process pipeline, performing both training and evaluation pipelines

For this use case, let's use our newly deployed training and evaluation datasets to run a training and evaluation pipeline. Proceed as follows:

1. Navigate to **Pipelines** by clicking the **Pipelines** tab in the project dashboard.
2. Click **Create New** to create a new pipeline, selecting the following:

    - **Pipeline type**: `Train run`
    - **Choose package**: `TextClassification`
    - **Choose package major version**: `6`
    - **Choose package minor version**: `0`
    - **Choose input dataset**: `Email Classification/train/`
    - **Enable GPU**: Turn this off

    Under **Environment Variable**, add the following environment variables:

    - `input_column`: `text`
    - `target_column`: `label`

![](img/05268897c8ac5d80eb3fead1cc8a822a_279_0.png)

Figure 9.10 – Creating a training pipeline

3. Click **Create** to start the training pipeline.

> **Important Note**
> The size of the dataset will affect the pipeline duration time. Choosing **Enable GPU** can lead to quicker output, but **graphics processing units (GPUs)** are not available with UiPath Community Edition.

4. Perform *Steps 2, 3, and 4* again, choosing `Evaluation run` as the pipeline type, version 6.1 of the `TextClassification` package, and the `evaluation` dataset folder as the input dataset. Your screen should now look like this:

![](img/05268897c8ac5d80eb3fead1cc8a822a_280_0.png)

Figure 9.11 – Creating an evaluation pipeline
