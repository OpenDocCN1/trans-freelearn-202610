# Hands-on exercise for adding Row Limit and the Dashboard Settings button

Let's consider a use case: creating a report that shows the top five Accounts with the most number of Employees and configuring the report as the data source for the dashboard:

+   1. Navigate to the **Reports** tab and click on the **New Report** button.
+   2. Select the **Accounts** Report Type and click on the plus sign under the **Accounts & Contacts** category.
+   3. Click on the **Create** button to continue.
+   4. Change **Show** to **All accounts**.
+   5. Change the **Date Field** range to **All Time**.
+   6. Remove unwanted default fields from the report and leave only **Account Owner** and **Account Name**.
+   7. Add the **Employees** field by dragging and dropping it into the report area.
+   8. In **Filters**, click on the arrow next to the **Add** button | **Row Limit**.
+   9. Change **Row Limit** to **5**, **Sorted By Employees - Descending**. Consider the following screenshot:

![](bbox=[0.1, 0.49, 0.89, 0.62])

+   10. Click on the **OK** button. You will notice the **Dashboard Settings** button appear at the top row buttons.
+   11. Click on the **Dashboard Settings** button.
+   12. Select **Account Name** for **Name** and **Employees** for **Value** and click on the **OK** button. Once the **Dashboard Settings** button is configured, this report will be able to use it as the data source in the dashboard component. We'll discuss dashboards in *Chapter 8, Working with Reports.*