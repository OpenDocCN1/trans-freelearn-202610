3. Now click on **Pipeline runs**. After some time, **Status** will change to **Succeeded**. This indicates a successful run:

![](bbox=[0.12, 0.15, 0.85, 0.33])

Figure 5.61 – The status of the Electroniz batch ingestion pipeline for the fourth iteration

4. Let’s validate whether the incremental data was correctly ingested. From the Azure portal, navigate to **Home | All resources | traininglakehouse.**

Navigate to **Containers | Bronze | sales**. Browse to the newly created folder from the last hour.

You should expect a new hour folder in the **[dbo].[store_orders]** folder, as follows:

![](bbox=[0.12, 0.5, 0.85, 0.73])

Figure 5.62 – New hourly folder for the fourth iteration