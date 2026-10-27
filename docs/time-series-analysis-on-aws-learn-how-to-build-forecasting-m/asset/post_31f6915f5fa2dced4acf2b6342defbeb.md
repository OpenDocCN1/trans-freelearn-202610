
Then, select **Enter a custom IAM role ARN** and paste the ARN of the `LookoutEquipmentIndustrialPumpDataAccess` role that you created in *Chapter 9, Creating a Dataset and Ingesting Your Data*, using the IAM service. The format of the ARN should be `arn:aws:iam::<ACCOUNT-NUMBER>:role/LookoutEquipmentIndustrialPumpDataAccess`, where `<ACCOUNT-NUMBER>` needs to be replaced with your AWS account number.

> **Important Note**
> 
> Although you will see this historical event file often called the *labels file*, this denomination has nothing to do with the type of machine learning algorithms that are used by Amazon Lookout for Equipment as they are all fully *unsupervised* ones. For more details about how this labels file is used, check out the *How is the historical events file used?* section, later in this chapter.

- 8. The **Training and evaluation settings** section will let you define the time split and the sample rate at which your data will be downsampled by the service:

![](img/e4ee48104c730845e796d8c469dfeecc_287_0.png)

Figure 10.8 – Defining the training and evaluation split

When you're configuring this section, you must define the following:

- a) **Training data time range:** This is the time range the service will use to train the model. For our industrial pump dataset, you can use `2018/04/01` to `2018/10/31`.

> **Important Note**
> 
> At the time of writing this book, the training range must at least be 90 days long. If your training range is shorter than this, the training process will fail.

- b) **Evaluation data time range:** This is the time range the service will use to evaluate a trained model. For our dataset, you can use `2018/11/01` to `2020/05/04`.

- c) **Time series sample rate:** You can either use the original sample rate of your dataset or request Amazon Lookout for Equipment to downsample your data with a custom interval that can go from 1 second to 1 hour. Let's use 5 minutes.

> **Important Note**
> 
> The sample rate choice impacts the training time – the smaller the sample rate, the longer the training will take. However, resampling your time series data also acts as a filter that keeps the highest frequency of your data out. If the anomalous events you are looking for are located in these higher frequencies, choosing a sample rate that is too coarse will filter them out and Amazon Lookout for Equipment will have a harder time finding them. If you have 6 to 12 months of data, a resampling rate of 5 minutes or 10 minutes is a reasonable starting point. Depending on the training time and the results of your model evaluation, you can retrain another model with a different sample rate and compare the output.

- 9. The last section of interest is the optional **Off-time detection** section. We will not use this section for this example. See the _Deep dive into the off-time detection feature_ section, later in this chapter, to get more details about this section.

- 10. Scroll down to the bottom of the page and click on the **Create** button. Your model training will be launched.

You should now have ongoing training in progress: based on the size of the dataset and the parameters that you have configured, it should take less than 1 hour to train your first anomaly detection model with Amazon Lookout for Equipment. In the meantime, you can read about the way historical events and off-time sensors are used.

## How is the historical event file used?

As a reminder, your historical event file (or labels files) looks like this:

|   | start               | end                 |
|---|---------------------|---------------------|
| 0 | 2018-05-30 01:30:00 | 2018-06-03 08:15:00 |
| 1 | 2018-06-24 14:25:00 | 2018-07-06 09:40:00 |
| 2 | 2018-11-27 04:25:00 | 2018-12-02 17:50:00 |
| 3 | 2018-12-26 14:25:00 | 2018-12-29 16:55:00 |
| 4 | 2019-06-19 01:55:00 | 2019-07-19 05:10:00 |
| 5 | 2019-08-03 12:50:00 | 2019-08-04 16:20:00 |
| 6 | 2019-10-30 09:55:00 | 2019-10-31 16:15:00 |

Figure 10.9 – Anomalous ranges overview

This label file provides insight into past events to Amazon Lookout for Equipment. Although all the algorithms that are used by the service are unsupervised, Amazon Lookout for Equipment uses these *optional* labels to train its models more accurately and efficiently. Leveraging its bank of more than 28,000 combinations of parameters and algorithms, Amazon Lookout for Equipment can use this label file to find the optimal model that finds abnormal behaviors within these time windows.

How does Amazon Lookout for Equipment use this data? Let's look at the first row and let's see how it's interpreted:

- - The second timestamp (**2018-06-03 at 8:15:00**) can be a maintenance event or when the abnormal behavior was noticed by someone.
- - The first timestamp (**2018-05-30 at 1:30:00**) indicates when the abnormal behavior is expected to have started.

## Deep dive into the off-time detection feature

This optional section lets you tell Amazon Lookout for Equipment which sensors it can use to decide that equipment is going through a shutdown. When your piece of equipment has long shutdown periods (regular or not), it is usually required to remove the signals from these periods as they are not relevant for finding any anomalies.

When creating a new model, the Off-time detection section is located at the bottom of the screen, before the Tags definition section:

![](img/e4ee48104c730845e796d8c469dfeecc_291_0.png)

Figure 10.12 – The Off-time detection feature

To use this feature, simply fill in the following fields:

- - **Sensor:** This dropdown list will be populated with the list of fields that you selected for this model. Each sensor will have a label of component name\tag name, as defined in the schema corresponding to the dataset that was used to train the model. In our case, we put each sensor in a component with the same name and each item of the dropdown list has a duplicated label (for instance, sensor_43\ sensor_43).

- - **Rule:** The rule can either be `less than` or `greater than`.
- - **Value:** Enter an integer or float value.

Let's say your equipment is a rotating machine and that `sensor_43` measures the rotation speed in RPM. You know that any rotation speed *less than 100 RPM* means your equipment is either off or currently shutting down. To tell Amazon Lookout for Equipment this, you will configure the **Off-time detection** section by writing a rule stating that `sensor_43\sensor_43` is `less than 100 RPM`.

Once a condition has been met, all the data satisfying it will be discarded to train a model. Similarly, at inference time, all the data satisfying this condition will be filtered out.

Now that you know how the labels data and the off-time detection feature are used to train an anomaly detection model, let's look at the different ways you can organize your collection of models.

## Model organization best practices

Amazon Lookout for Equipment includes the following hierarchy of artifacts within a given AWS account:

- - The **dataset** is the highest level of the hierarchy; a dataset is defined by an immutable data schema that's defined at creation time.
- - When defining the data schema, the different tags can be regrouped in different **components**: a component must match a folder in your S3 training dataset.
- - Each dataset can be used to train multiple **models** and each model can use all the sensors available in the dataset (as defined in the schema) or only a selection of those.
- - The lower level of this hierarchy is the **sensor** time series (also called **tag**, **signal**, or **field**).

> **Note**
> A higher level of this hierarchy is the AWS account/user. Although more heavy lifting will be required to set up the appropriate permission, you can build a solution where multiple AWS accounts would use Amazon Lookout for Equipment across your organization, depending on their geographical location, for instance.
