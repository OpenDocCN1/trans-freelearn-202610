
# 7

## Adding Charts in Reports and Pages

Starting from Chapter 4, Creating and Managing Reports, you learned to create and manage reports, continued with report types in Chapter 5, Understanding Report Types, and then used bucket fields and Custom Summary Formula in Chapter 6, Advanced Report Configuration.

In this chapter, we will continue by adding a chart to a report, and we will discuss multiple types of charts available in Salesforce Lightning and the types of report formats that support the addition of a chart, and we will walk through at a hands-on exercise on adding and working with charts in a report.

Next, we will discuss how to add the chart from the report to the record page, for example, opportunities or cases related to an account in an account page; the chart here will be filtered by the account opened, and we will also cover a hands-on exercise on this.

By end of this chapter, you will be fully equipped with knowledge of the types of charts in a report and will learn how to add and edit charts. You will also understand how to add charts in a report to the record page layout.

Throughout this chapter, we will provide notes and tips to help you understand important items. The following topics will be covered:

- Types of charts in a Lightning report
- Adding a chart to a report
- Adding embedded charts to the record page

## Adding Charts in Reports and Pages

Each chart format has its own extra features, such as an inline chart, and you can enable the cumulative value in a stacked bar chart, you can set all bar lengths to 100%, and so on.

Let's understand each type of chart available in Salesforce.

### The Horizontal Bar chart

The Horizontal Bar chart shows values as horizontal lengths, so this format is good for comparing values. Select a Horizontal Bar Chart format when you have many values on the y-axis, for example, a pipeline report based on **Sum of Amount** and **Sum of NRR Amount** for each sales representative. If we have 20 sales representatives, the charts will still look good and not be too crowded.

When you hover your mouse above the chart, it will show you more detailed information of the bar, including the x-axis value, the y-axis value, and the percentage of the bar compared to the total. See the following screenshot for a Horizontal bar chart, including information popup when you hovering the mouse over a bar:

![](img/72d01bea5a9ccf02f9452aab7505d9de_205_0.png)

![](img/72d01bea5a9ccf02f9452aab7505d9de_207_0.png)

With a Stacked Bar Chart, you will have the option to set Stack to 100%; by enabling this, all bars will have the same length. The preceding Horizontal bar chart if modified to Horizontal Stacked Bar chart will be as shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_207_1.png)

### The Stacked Vertical Bar chart

The Stacked Vertical Bar chart is pretty much the same as the Stacked Horizontal Bar chart. It will change the bar to vertical, and the x-axis and the y-axis are swapped. The following screenshot is the same as the preceding one, but is a Vertical stacked bar chart:

![](img/72d01bea5a9ccf02f9452aab7505d9de_208_0.png)

### The Line chart

A Line chart is good for showing changes in the values of an item over a series of points in time. Following is a sample of Opportunity line chart grouped by Opportunity Owner:

![](img/72d01bea5a9ccf02f9452aab7505d9de_209_0.png)

You can configure a Line chart to show as Cumulative; this is a good option when you would like to show the total number of items over a period of time. The following screenshot shows a Line chart with cumulative values:

![](img/72d01bea5a9ccf02f9452aab7505d9de_210_0.png)

### The Donut chart

With the Donut chart, the user is able to see the biggest portion and the smallest portion at a glance; on top of that, it shows the total amount and the amount for each group, for example, the pipeline report by stage with the total value. Just like the stacked bar chart, when you hover your mouse over the group, it will show you the group name and the group amount. The following sample shows a Donut chart with groups by month of Close Date:

![](img/72d01bea5a9ccf02f9452aab7505d9de_211_0.png)

## Adding Charts in Reports and Pages

![](img/72d01bea5a9ccf02f9452aab7505d9de_213_0.png)

### Hands-on exercise for adding a chart to a report

In this exercise, we will create an **Opportunity** report group by Opportunity **Stage** in the **Summary** format. Let's walk through the steps:

- 1. Navigate to the **Reports** tab and click on the **New Report** button.
- 2. Select the **Opportunities** report type and click on the plus sign under the **Opportunities** category.
- 3. Click on the **Create** button to continue.
- 4. Change **Show** to **All opportunities**.
- 5. Change the **Date Field Range** to **Previous FY**.
- 6. Click on the **Remove All Columns** link to clear all default fields added to a report and then click on the **OK** button to confirm.
- 7. Change the report format to **Summary**.
- 8. Drag these fields into the report: **Opportunity Name**, **Amount**, and **Expected Revenue**. To find the field name quickly, type the field name in the field textbox and double-click on the field to get it added to the report.
- 9. Drag the **Stage** field to the grouping drop zone.
- 10. Summarize **Amount** and **Expected Revenue** with **Sum**.
- 11. Click on **Show** and then click on **Details** to hide the report details.
- 12. Click on the **Save** button and enter the following information:
  - Report Name is `Previous FY Opportunities by Stage`
  - Report Folder is **My Personal Custom Reports**
  - Click on the **Save** button
  - Click on the **Run Report** button
- 13. You should see something similar to what is shown in the following screenshot; this is a standard Salesforce report:

![](img/72d01bea5a9ccf02f9452aab7505d9de_214_0.png)

14. Now we are going to add a chart to this report; click on the pie chart icon in the top-right section next to the report name; by default, it will generate a Horizontal bar chart:

![](img/72d01bea5a9ccf02f9452aab7505d9de_215_0.png)

15. If you are happy with this chart, click on **Save** under the arrow next to the **Edit** button:

![](img/72d01bea5a9ccf02f9452aab7505d9de_216_0.png)

16. Let's customize this chart as a **Donut** chart.

17. Click on the gear icon, the one just above the chart on the right-hand side, not the gear icon next to the **Edit** button:
  - Under **Display As**, select **Donut**.
  - Add **Chart Title** as `Previous FY Opportunity`.
  - Under **Value**, we can select **Sum of Amount**, **Sum of Expected Revenue**, and **Record Count**; remember that in Step 10, we use the sum for both fields; for now, just leave it as **Sum of Amount**.
  - Under **Sliced By**, you can change the chart color by clicking on the color icon.
  - By default, **Show Values** is ticked; let's leave it as it is.
  - You can select to display **Legend Position** on the right-hand side or at the bottom. This is shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_217_0.png)

18. Click anywhere in the chart to show the result. Remember to save the report. This will include the chart that we just configured; otherwise, the added chart will not be stored and will not be there when you or someone else opens the report in the future.

19. Now let's save the report as a new report and change the chart format to **Line**.

20. Click on the **Edit** button, click on the **Save As** button, Report Name as `Previous FY Opportunities by Stage /Line`, and continue by clicking on the **Save and Run Report** button.

21. Click on the gear icon in the report; if you hover over the icon, it will show a pop-up label--**Edit Chart**.
