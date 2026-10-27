
16. Click on Save from the drop-down arrow to save all changes in the report. With this, you can save the report without the need to edit it from report builder:

![](img/72d01bea5a9ccf02f9452aab7505d9de_98_0.png)

## The Matrix report format

The Matrix report is pretty similar to the Summary report, but with the additional capability to group by both rows and columns. You also can have up to two levels of grouping for both rows and columns.

Similar to the Tabular and Summary report format, you can add summarizable fields in Matrix reports. By default, the record count is added into the report matrix. When you add the summary field in the Matrix report, the field and summary will be shown in the matrix, not at the bottom of the column as in the Summary or Tabular report format. Once you add a summary field to the Matrix report, you can remove the default record count from the matrix if you don't need it.

This report's format is good when you have large amounts of data to summarize and need to compare records with different values in the same field with the Matrix report. The Matrix report, without at least one row and one column grouping, will be shown as a Summary report when you run the report.

You can change from the **Tabular** or **Summary** report to the **Matrix** report when you change a report from **Summary** to **Matrix** format:

- First-level grouping will automatically become first-level row grouping
- Second-level grouping will automatically become first-level column grouping
- Third-level grouping will automatically become second-level row grouping

In the following example, we use the same summary format report created earlier in **All Account by creation Year and owner**; the report shows the same data and criteria used--just in a different format. Note that the record count is added by default when you create a **Matrix** report. The following screenshot shows us a **Matrix** report converted from **Summary** format report:

![](img/72d01bea5a9ccf02f9452aab7505d9de_99_0.png)

## Hands-on exercise for creating a Matrix report

Let's consider a use case: showing the record count for all accounts in a **Matrix** format, row grouping by Account creation for **Calendar Year**, and column grouping by **Account Owner**:

- 1. Instead of creating it from scratch, let's modify the existing **Summary** report `All Account by creation Year and owner` and convert it into a **Matrix** report.
- 2. Open the **All Account by creation Year and owner** report and click on **Edit** button or navigate to **Reports** tab, look for the report, click on the arrow in the last column, and then click on **Edit** to open report builder.
- 3. Change the report format from **Summary** to **Matrix**.
- 4. The first group of the summary report will automatically become row grouping, while the second group will become column grouping; in our exercise, **Created Date** will be a row grouping and **Account Owner** will be a column grouping.

Let's take a look at the following screenshot and notice that the first-level and second-level grouping of the Summary report has been transformed:

![](img/72d01bea5a9ccf02f9452aab7505d9de_100_0.png)

When the report format changes from **Summary** to **Matrix** report:

![](img/72d01bea5a9ccf02f9452aab7505d9de_101_0.png)

5. Since this report was originally saved as a Summary report, you can just click on the Run Report button to see the report result in a Matrix format. In Lightning Experience, a report needs to be saved before you are able to run it:

![](img/72d01bea5a9ccf02f9452aab7505d9de_101_1.png)

6. If you just need to see the report summary without any detail, click on the gear icon next to the refresh icon (not the topmost gear icon with lightning inside the gear) and then uncheck the Details checkbox. Often, we need to do this when we present the report and want to focus on the summary only, without the need to see the details, as shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_102_0.png)

7. Here is the report result which just shows the summary based on Created Date and Account Owner in a Matrix format:

![](img/72d01bea5a9ccf02f9452aab7505d9de_102_1.png)

- 8. If you click on **Save** from here, this will overwrite your **Summary** report created earlier. So instead of clicking on Save, click on **Clone** to save the report as a new report, enter the new report name, select the folder, and then click on the **Create** button in the pop-up window to confirm.
- 9. Alternatively, when you are in report builder in step 5, instead of clicking on the **Run Report** button to run the report, click on the **Save As** button to store the report as a new report.

## Hands-on exercise for adding a Summary field to Matrix reports

Let's consider a use case: showing all opportunities with the total amount in a **Matrix** format, row grouping by **Opportunity Stage**, and column grouping by **Opportunity Owner**:

- 1. Navigate to the **Reports** tab and click on the **New Report** button.
- 2. Select the **Opportunities** Report Type and click on the plus sign under the **Opportunities** category.
- 3. Click on the **Create** button to continue.
- 4. Change **Show** to **All opportunities**.
- 5. Change the **Date Field** range to **All Time**.
- 6. Note that the **Opportunity Status** and **Probability** filters are next to **Show**; these specific filters are there only when we use the **Opportunity** Report Type. For this exercise, just leave the default value **Any** for **Opportunity Status** and **All** for **Probability**.
- 7. Change the report format from **Tabular** to **Matrix**.

8. Remove all fields by dragging and dropping them out to the Fields area except for Opportunity Name, Amount, Stage, and Opportunity Owner fields. Now you should see something similar to what is shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_104_0.png)

9. To sum the amount, hover your mouse over the Amount header, click on the arrow icon in Summarize this Field..., select Sum, and click on the Apply button as shown in the following screenshots:

![](img/72d01bea5a9ccf02f9452aab7505d9de_104_1.png)

![](img/72d01bea5a9ccf02f9452aab7505d9de_105_0.png)

If the field you would like to summarize is not added to the report yet, you can drag the summarizable fields (such as the currency, number, and percentage) directly into the matrix, and the system will prompt you to summarize by Sum, Average, Max, or Min.

You can change the field order added to the matrix by dragging and dropping the summarized field. To remove the summarized field from the matrix, you can just simply drag it out to the field area in the left-hand side panel.

- 10. Now you should see the amount and its value in a matrix; refer to the screenshot in Step 11.
- 11. Drag **Stage** as the row grouping and **Opportunity Owner** as the column grouping as shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_105_1.png)

- 12. Note that you can drag another field as a level 2 grouping for both the row and column grouping.
- 13. As we just want to focus on the summary value, let's hide the report detail by clicking on **Show** next to the **Remove All Columns** link and then unchecking **Detail**.

- 14. Next, let's also remove the record count as we just need the amount value. Click on **Show** again next to the **Remove All Columns** link and then uncheck **Record Count**. Remember that we can only remove the record count when a field has been added as the summarized field in the matrix.

- 15. Click on the **Save** button; this will open a pop-up window to save the report:
  - Enter **Report Name** as `Opportunity by Stage and Owner`
  - **Report Unique Name** will be auto-filled
  - It is always a good practice to enter some **Description** for future reference
  - Select **My Personal Custom Reports** in **Report Folder**, so no one can access it
  - Click on the **Save** button to save the report

- 16. Click on **Run Report**, and you should see a report similar to the following report:

| OPPORTUNITY OWNER | ALVIN LEE | JOHAN YU | Total |
|---|---|---:|---:|---:|
| **STAGE** | **AMOUNT Sum** | **AMOUNT Sum** | **AMOUNT Sum** |
| Prospecting | $0.00 | $80,000.00 | $80,000.00 |
| Qualification | $0.00 | $115,000.00 | $115,000.00 |
| Needs Analysis | $675,000.00 | $0.00 | $675,000.00 |
| Value Proposition | $0.00 | $330,000.00 | $330,000.00 |
| Id. Decision Makers | $0.00 | $110,000.00 | $110,000.00 |
| Perception Analysis | $0.00 | $120,000.00 | $120,000.00 |
| Proposal/Price Quote | $270,000.00 | $100,000.00 | $370,000.00 |
| Negotiation/Review | $395,000.00 | $0.00 | $395,000.00 |
| Closed Won | $0.00 | $3,716,000.00 | $3,716,000.00 |
| **Total** | **$1,340,000.00** | **$4,571,000.00** | **$5,911,000.00** |
