15. Now, chain **Create Synapse Credentials** to **Create Silver Layer Views**. Once you've done this, the pipeline should look like this:

![](bbox=[0.14, 0.14, 0.86, 0.36])

Figure 8.21 – Chaining activities – Create Synapse Credentials to Create Silver Layer Views

16. In the **Activities** panel, search for Delete. Now, drag the **Delete** activity to the right-hand panel and provide the following properties:

+   - **Name:** `Cleanup Gold Data.`
+   - **Click on Source.** Now, click on **New**.
+   - **Choose Azure Data Lake Storage Gen 2** and click **Continue**.
+   - **Name:** `gold_data.`
+   - **Linked service:** `traininglakehouse.`
+   - **File path:** `gold/external:`

# Set properties

![](bbox=[0.18, 0.63, 0.82, 0.79])

Figure 8.22 – Properties of the Delete activity