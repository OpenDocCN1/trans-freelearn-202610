Click on **Show Secret Value** and validate that it shows the Azure Data Lake Storage key:

![](bbox=[0.19, 0.15, 0.775, 0.342])

Figure 5.27 – Creating a new secret in Azure Key Vault

Note down the value of the **secret identifier**. You will need this later.

5. Finally, we want to provide Azure Data Factory access to read the secrets.

Navigate to **Home | All Resources | trainingkv100 | Access Policies.**

Then, click on **Add Access Policy.**

For Key permissions, choose **Get** and **List.**

For Secret permissions, choose **Get** and **List.**

For Select Principal, click on **None Selected.** For **Principal**, find the name of your Azure data factory created earlier:

![](bbox=[0.185, 0.59, 0.781, 0.874])

Figure 5.28 – Choose a principal for access to a new secret