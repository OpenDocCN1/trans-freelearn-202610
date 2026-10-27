
3. CloudFormation is a service used to deploy any AWS service with a template written in either JSON or YAML format. This allows you to configure services with code instead of building everything by clicking through multiple service consoles. Click on the **Next** button on the bottom right.

![](img/e4ee48104c730845e796d8c469dfeecc_222_0.png)

Figure 7.11 – Creating the CloudFormation stack

4. On the stack details page, enter a stack name. I chose `energy-consumption-forecast-stack` for mine.

![](img/e4ee48104c730845e796d8c469dfeecc_222_1.png)

Figure 7.12 – CloudFormation stack name

5. In the stack parameters, you will fill in your email address in **Email**, leave the QuickSight visualization option empty, and select **Yes** to deploy a Jupyter notebook. Leave all the other options as is and click **Next**.

![](img/e4ee48104c730845e796d8c469dfeecc_223_0.png)

Figure 7.13 – CloudFormation parameters

6. On the next screen, you will have the opportunity to configure advanced options. You can leave all these values as their defaults, scroll down the screen, and click **Next** again.

7. On the last screen, you can review all the options and parameters selected and scroll down to the very bottom. Click on both the check-boxes and click on Create stack.

![](img/e4ee48104c730845e796d8c469dfeecc_224_0.png)

Figure 7.14 – Creating the CloudFormation stack

8. This will bring you to a CloudFormation stack creation screen where you can see your stack deployment in progress.

![](img/e4ee48104c730845e796d8c469dfeecc_224_1.png)

Figure 7.15 – Stack creation started

9. At the end of the process, you can navigate to the Outputs tab of the template deployment. Take note of the ForecastBucketName value under this tab.

![](img/e4ee48104c730845e796d8c469dfeecc_225_0.png)

*Figure 7.16 – CloudFormation stack outputs*

10. Once you're through the CloudFormation stack creation, you will also receive an email asking you to confirm your subscription to a notification topic. This SNS topic will be used to send you notifications about the forecasting process. Click on the **Confirm subscription** link in this email.

![](img/e4ee48104c730845e796d8c469dfeecc_225_1.png)

*Figure 7.17 – Notification subscription confirmation*

Your solution is now deployed. It's time to configure it to match our energy consumption prediction use case.

## Configuring the solution

The *Improving Forecast Accuracy with Machine Learning* solution is configured with a YAML file that you will need to upload to the S3 bucket you took note of earlier (*Step 9 of the Solutions deployment section*). The file you will use will contain an introduction section configured as follows:

```
Household_energy_consumption:
  DatasetGroup:
    Domain: CUSTOM
```

You then configure the different datasets to be used. Here is the section for the target time series dataset:

```
Datasets:
  - Domain: CUSTOM
    DatasetType: TARGET_TIME_SERIES
    DataFrequency: D
    TimestampFormat: yyyy-MM-dd
    Schema:
      Attributes:
        - AttributeName: item_id
          AttributeType: string
```

Next, you will describe how the predictor will be trained:

```
Predictor:
  MaxAge: 604800
  PerformAutoML: False
  ForecastHorizon: 30
  AlgorithmArn: arn:aws:forecast:::algorithm/CNN-QR
  FeaturizationConfig:
    ForecastFrequency: D
  TrainingParameters:
    epochs: "100"
    context_length: "240"
    use_item_metadata: "ALL"
    use_related_data: "FORWARD_LOOKING"
```

Last but not least, you will define which forecast types you are interested in:

```
Forecast:
  ForecastTypes:
    - "0.10"
    - "0.50"
    - "0.90"
```

You will find a copy of the complete YAML file in the archive downloaded earlier.

Navigate to the Amazon S3 bucket you took note of earlier and upload this YAML file to this bucket. Also, click on Create folder and create a folder named train. Your S3 bucket root looks like the following (a folder named glue was created during the initial deployment and should already be present):

![](img/e4ee48104c730845e796d8c469dfeecc_227_0.png)

Figure 7.18 – S3 bucket initial configuration

Your solution is now configured and ready to be used!

## Using the solution

After deploying the Improving Forecast Accuracy with Machine Learning solution, you are now ready to use it. In this section, you will learn how to trigger and monitor the main workflow run by the solution and how to visualize the results.

### Monitoring the workflow status

Navigate to the train directory of your S3 bucket and upload the three CSV files you extracted in the Data preparation component section.

3. You will land on a page listing all the workflows visible from your AWS account. If this is your first time using the service, your list will only contain the state machine deployed by the solution.

![](img/e4ee48104c730845e796d8c469dfeecc_229_0.png)

Figure 7.20 – Step Functions state machine list

4. Click on the Improving-Forecast-Accuracy-energy-consumption-stack link to visualize all the executions of this workflow. You should see three ongoing executions, one for each file uploaded. Click on one with a Running status.

![](img/e4ee48104c730845e796d8c469dfeecc_229_1.png)

Figure 7.21 – State machine executions list

5. You will see the workflow status with the current step (Import-Data) and the past successful ones (Check-Error and Create-DatasetGroup) highlighted.

![](img/e4ee48104c730845e796d8c469dfeecc_230_0.png)

Figure 7.22 – Graph inspector for the current workflow execution

This graph is updated live and is useful for debugging any issues happening in the process. Once you're familiar with the solution, you usually don't need to open the Step Functions console anymore.

### Visualizing the results

Once all the workflows are run successfully, you will receive a notification by email and you can then visualize your predictor results. Follow these steps to this effect:

- 1. Log into your AWS console and search for SageMaker in the search bar at the top.
- 2. Click on **Amazon SageMaker** to go to the service home page. On this page, click on the **Notebook instances** link in the left menu bar.

![](img/e4ee48104c730845e796d8c469dfeecc_231_0.png)

Figure 7.23 – Amazon SageMaker home page

3. You will land on a page listing all the notebook instances visible from your AWS account. If this is your first time using the service, your list will only contain the notebook instance deployed by the solution.

![](img/e4ee48104c730845e796d8c469dfeecc_232_0.png)

Figure 7.24 – Amazon SageMaker notebook instances list

4. Click on the Open JupyterLab link to access your instance; you will be redirected to the JupyterLab environment. On the left, you have a directory structure. Drag and drop the .ipynb file you downloaded earlier at the beginning of this section to this directory structure area. Then, double-click on the EnergyConsumptionVisualization.ipynb notebook name to open it in the main interface on the right.

![](img/e4ee48104c730845e796d8c469dfeecc_232_1.png)

Figure 7.25 – Using a Jupyter notebook for visualization purposes

5. Jupyter notebooks are structured in successive cells. Update the first cell so that your code matches the name of your dataset group and the location of the forecast exports:

```
START           = '2013-06-01'
END             = '2013-07-31'
DATASET         = 'household_energy_consumption'
FORECAST_PREFIX = 'export_energy_consumption_XXXX'
```

6. From the top menu bar, select Run and then Run all cells to run the whole notebook.

7. Scroll down to the bottom of the notebook to visualize some of the results. For instance, here are the results for the MAC001259 household:

![](img/e4ee48104c730845e796d8c469dfeecc_233_0.png)

Figure 7.26 – Visualizing energy consumption for a household

## Cleanup

Once you are done experimenting with this solution, you can delete all the resources created to prevent any extra costs from being incurred. Follow these steps to clean your account of the solution resources:

- 1. Log into your AWS console and search for `CloudFormation` in the search bar at the top.
- 2. Click on **AWS CloudFormation** to go to the service home page. Click on **Stacks** in the left menu bar to see the list of CloudFormation stacks deployed in your account.

![](img/e4ee48104c730845e796d8c469dfeecc_234_0.png)

Figure 7.27 – AWS CloudFormation service home page

- 3. In the search bar at the top of the **Stacks** table, you can enter `energy-consumption` if you don't find your stack right away.
