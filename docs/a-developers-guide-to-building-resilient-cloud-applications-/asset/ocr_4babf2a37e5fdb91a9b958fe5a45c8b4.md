+   4. Leave the default value as-is for the **Networking** tab. We can configure a private or public endpoint and choose whether or not to enable a managed virtual network. Under **Advanced**, we can enable encryption using a customer manager key to secure our ADF instance further.

+   5. Select **Review + create**, and then select **Create** after the validation has passed.

Once the ADF resource has been created, we must navigate to the ADF page and start exploring the different settings. Select **Open Azure Data Factory Studio**; a new page will be loaded in a separate tab in your browser:

![](bbox=[0.13, 0.264, 0.86, 0.61])

Figure 10.9 – Opening ADF Studio in the ADF service

To start with ADF, we will create a linked service, datasets, and pipeline.

# Creating a linked service

We need to follow these steps:

+   1. To create a linked service to link a data store to our ADF instance, we can select any external store or Azure Storage, or a database, file, or NoSQL database. Select the **Manage** tab from the left pane and after that, select **+ New**: