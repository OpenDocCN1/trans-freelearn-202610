![](bbox=[0.177, 0.091, 0.822, 0.377])

Figure 8.27: Setting the Summarize by property to None for all columns across the model

The second method is to disable the implicit measures using **Tabular Editor** across the entire model. This method prevents the model from detecting implicit measures; therefore, if we add more tables to our data model in the future, the model will not see any more implicit measures. **Tabular Editor** is an external tool that’s widely used by the community. This setting is not available in Power BI Desktop at the time of writing. The following steps explain how to disable implicit measures from Tabular Editor:

+   1. Click the **External Tools** tab from the ribbon in Power BI Desktop.
+   2. If you installed the latest version of **Tabular Editor**, it would appear in **External Tool**. Click **Tabular Editor**.
+   3. In **Tabular Editor**, click **Model**.
+   4. Set the values of the **Discourage Implicit Measures** option to **True**.
+   5. Save the changes you made to the model: