Some points to note in relation to the embedded chart in the record page are as follows:

+   - To add the chart to the page layout, you need to edit the page layout so only admins or users with the **Customize Application** permission are able to perform this
+   - You can only add up to two charts in a page layout
+   - The chart shown on the record page is based on the chart added in the **Summary** or **Matrix** report
+   - You cannot modify the chart type in the page layout
+   - You can set the chart to auto-refresh when the user opens the page layout
+   - When the user clicks on the chart in the page layout, it will drill down to the data by opening the report and automatically filtering the report based on the record where the user opens the record.

# Hands-on exercise for adding a chart to a page layout

Let's consider a use case: adding an embedded chart of opportunity and the number of cases in the **Account** page layout.

+   1. Create an **Opportunity** report, group the report by **Stage**, add **Amount**, summarize it, and save the report to a public folder, add a **Vertical Bar Chart**, and save the report again.
+   2. Create a case report from the **Custom Support Reports** report type category, group the report by **Status**, save the report to a public folder, add a **Donut Chart**, and save the report again.
+   3. Edit the account page layout by navigating to **Setup | Objects and Fields | Object Manager | Account | Page Layouts**. Open the corresponding page layout and look for **Report Charts** in the left-hand side menu. Click on **Report Charts** and find the report in the right-hand side panel; only the report with the chart will be shown here, and stored in the public folder: