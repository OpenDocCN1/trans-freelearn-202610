
![](img/dae8a2243a3c3cbcd9c56ff3d2217672_355_0.png)

Figure 10.23 – State of data sharing currently

Unfortunately, there are several problems related to these data-sharing methods:

- **Complex**: These data sharing mechanisms can be complex to set up and use because they may require exchanging keys/passwords and using a variety of different tools.
- **Insecure**: These mechanisms may not be secure for data-at-rest or data-in-transit. This means the classic **man-in-the-middle** attack could expose data in cleartext.
- **Tracking**: There is no clear method available for effectively tracking who shared data with whom and using which mechanism.
- **Control**: You can't control over who is using the shared data.
- **Updates**: Sharing data is a continuous process, and using the mechanisms mentioned previously does not provide a simple way to share updates.

Fortunately, there is an effective alternative. Azure allows organizations to share data both internally and externally using a simple and secure service named **Azure Data Share**. Using Azure Data Share, users sharing data (**providers**) can easily invite users they wish to share data with (**consumers**). Before the data is shared with the consumers, they are required to accept the data usage terms. Updates to data can also be shared on a set frequency that's been configured by the provider. Data sharing can happen in one of two ways. We will look at both in the following subsections.

## In-place sharing

In this case, data always stays in the provider's Azure subscription, and a symbolic link is sent to the consumer's Azure subscription. Consumers can read the data using this symbolic link, as shown here:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_356_0.png)

Figure 10.24 – In-place sharing using Azure Data Share

## Snapshot sharing

In this case, data moves from the data provider's Azure subscription and lands in the data consumer's Azure subscription:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_356_1.png)

Figure 10.25 - Snapshot sharing using Azure Data Share

Let's learn more about sharing data by performing a simple exercise. For this exercise, you will need a sharing buddy to act as the consumer. Your buddy should have an operational Azure account. If they don't, they can create a new one.

## Preparing the Azure resources

We will start by preparing the Azure resources that are required for sharing data:

- 1. On the Azure portal, search for Data Shares and navigate to it.
- 2. On the Data Shares page, click Create.
- 3. On the Create Data Share page, enter the following details:
  - Subscription: Free Tier or Paid Subscription
  - Resource group: training_rg
  - Location: East US 2
  - Name: trainingshare
Click on Review + create:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_357_0.png)

Figure 10.26 – Creating a new data share

- 4. Notice the Validation Passed message at the top. Press Create.
- 5. The newly created data share will now be deployed. After the deployment finishes, you should receive a message stating Your deployment is complete.

6. Click on Go to resource. This should take you to the newly created data share page:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_358_0.png)

Figure 10.27 – Home screen of the new data share

> **Important**
> The decision of sharing data internally or externally is extremely sensitive. As a data engineer, you should mandatorily seek written approval from the customer before proceeding with the data share.

## Creating a data share

Now that we have created a new data share account, we can proceed with sharing the data:

- 1. Click on Start sharing your data. Now, click on Create and provide the following values:
  - Share name: golddata
  - Share type: Snapshot

Click Continue. This should take you to the Datasets tab. From here, click on Add datasets and choose Azure Data Lake Gen2:

Figure 10.28 – Choosing a dataset type for the data share

2. Click Next and provide the following values:
  - **Subscriptions:** Free Tier or Paid Subscription
  - **Resource groups:** training_rg
  - **Storage accounts:** traininglakehouse:

Figure 10.29 – Choosing an Azure Data Lake Storage account for the data share

3. Click Next.

On the Select datasets page, choose gold and click Next:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_360_0.png)

Figure 10.30 – Choosing the gold dataset for the data share

4. On the Rename datasets page, leave the name as is or change it as required:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_360_1.png)

Figure 10.31 – Renaming the dataset for the data share

5. Click Add datasets and then Continue.

Now, we will add the email address value of the consumer (buddy) who will receive the share.

6. Click on **Add recipient**. Enter the email address of the recipient and click **Add**, followed by **Send invitation**:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_361_0.png)

Figure 10.32 – Adding the email address of the consumer

7. Azure will now send an email to the consumer, who will need to accept the invitation and execute a series of steps to download data from the newly created data share.

Please note that the next series of steps are from the point of view of the consumer. Some screenshots displayed hereafter will reflect what the consumer will see after they receive the email. To show this difference, all the screenshots with a **white background** are from the consumer's Azure account.

8. The consumer will receive an email that looks like this:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_361_1.png)

Figure 10.33 – Invitation email for the consumer

9. After clicking View Invitation, the request will be forwarded to the consumer's Azure portal. After logging into the portal, the following screen will appear:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_362_0.png)

Figure 10.34 – The customer viewing the invitation

Click on golddata and provide the following values:
  - Data share account name: golddatashare
  - Subscription: Free Tier or Paid Subscription
  - Resource group: training_rg
  - Location: East US 2

Click on Create. You will receive the following error:

The subscription is not registered to use namespace 'Microsoft.DataShare'. See https://aka.ms/rps-not-found for how to register subscriptions.

There's no need to panic – Azure subscriptions do not enable Data Sharing by default.

10. Navigate to the Subscriptions page and click on your Azure subscription:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_363_0.png)

Figure 10.35 – Enabling data sharing in the consumer's subscription

11. Now, click on Resource providers using the menu on the left:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_363_1.png)

Figure 10.36 – Navigating to the resource providers in the consumer's subscription

12. Using the search bar at the top, type datashare. Notice that Microsoft.Datashare is currently not registered:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_364_0.png)

Figure 10.37 – Checking the state of Microsoft.Datashare in the consumer's subscription

13. Choose Microsoft.Datashare and click on Register. Microsoft.Datashare will now be registered for the consumer's Azure subscription. Please note that this step may take a few minutes to complete:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_365_0.png)

Figure 10.38 – Registering Microsoft.Datashare in the consumer's subscription

14. Once the status changes to Registered, navigate back to the data share account tab and click on Create again. Hopefully, this time, the process will run without any errors:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_365_1.png)

Figure 10.39 – Creating a new data share in the consumer's subscription

15. The data share invitation will now be displayed. Enter the details shown in the following screenshot, and then click Accept and configure:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_366_0.png)

Figure 10.40 – Setting the data share parameters in the consumer's subscription

16. The confirmation screen for a successful data share should look like this:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_366_1.png)

Figure 10.41 – Home screen of the data share in the consumer's subscription
