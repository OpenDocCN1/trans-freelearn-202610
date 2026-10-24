+   • The letter N was dropped from the word **Net** in **Column2** and **Column3** on rows **61**, **66**, and **67**, as shown in **Figure 12.19**. In my testing, I found this doesn’t happen to everyone, but it happened on my computer, so I’ll show you how to fix it in case you encounter this oddity:

![](bbox=[0.133, 0.18, 0.854, 0.393])

+   • Some amounts appear in **Column6**, while others appear in **Column7**, as shown here:

![](bbox=[0.133, 0.474, 0.854, 0.687])

+   • **13,68553** appears in **Column7** of row **67** instead of **13,685.53** because the decimal is missing. If you click the **ABC123** icon in the heading for **Column7** and choose **Currency**, **Power Query** will translate this number as **13,685,533**.

Fortunately, we can use **Power Query** features to resolve all four of the preceding issues.

+   8. Click on **Column1** and then hold down the **Shift** key while you click on **Column5**.

+   9. Right-click on the selection and choose **Merge Columns** (or click **Transform | Merge Columns**).