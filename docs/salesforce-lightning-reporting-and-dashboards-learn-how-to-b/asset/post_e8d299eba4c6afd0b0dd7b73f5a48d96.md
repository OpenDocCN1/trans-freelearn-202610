
## 10 Learning Advanced Dashboard Configuration

In Chapter 9, Building Dashboards in Lightning Experience, we introduced dashboards, starting with opening a dashboard, navigating to the Dashboards tab, navigating to a dashboard, and looking at the items in the Dashboard Builder. You learned the basic structure of a dashboard and saw how it is related to the report, and how a dashboard is defined and stored on the Salesforce platform. We continued with the dashboard components, which are the heart and soul of dashboards in Salesforce. Finally, we created a dashboard from scratch and then enhanced it with dashboard filters and scheduled the dashboard for auto-refresh.

In this chapter, you will continue to learn and create an advanced dashboard using out-of-the-box functions offered by Salesforce. We will start with the dashboard filter and cover how to implement it and other use cases for the dashboard filter. We will continue with the concept of dynamic dashboards, and we will look into the dashboard drill down with filters preserved from the dashboard to report. We will end with looking for a prebuilt dashboard available in AppExchange and cover a hands-on exercise to install it.

The following topics will be covered in this chapter:

- Implementing the dashboard filters
- Using dynamic dashboards
- Dashboards and reports drill down
- Getting more from AppExchange

### Implementing the dashboard filters

Have you ever had the need to analyze a dashboard based on different criteria? For example, we would like to look at the pipeline dashboard for the current year, current quarter, and current month.

For this example, it is not an issue to create three dashboards, but since the data source of the dashboard is driven by reports, this means that we would need to create three sets of reports for each dashboard, and if you have 10 components for the dashboard, you will need 30 reports to support this.

Therefore, instead of creating multiple dashboards, it would be better to consolidate and easier for the user to just have one dashboard only since all the components for the dashboard are the same. Also, consider the maintenance effort for 30 reports and three dashboards and compare this to one dashboard with 10 reports used as the data source.

The dashboard filter comes into the picture for this scenario; with just one dashboard, you can change the filter to include different records in the dashboard based on the selected filters.

### Getting to know the dashboard filters

The dashboard filter is a great feature to filter the data in the dashboard component. All components will be filtered according to the filters selected. With the dashboard filter, you can use the same dashboard with different combinations of data.

Adding filters to a dashboard gives users the ability to have many results using a dashboard, so one dashboard serves the purpose of multiple dashboards. Without filters, you’d have to create multiple dashboards, with each dashboard having the same components with different filters. For example, add a filter in the industry field to track an account by industry. As admin, it is easier to maintain fewer dashboards.

If you have selected filters on a dashboard, the last filters selected by you will be applied by default when you reopen the dashboard. Salesforce will remember this for each user filter it applied for each dashboard.

To create a dashboard filter, you define a label for the filter, select the field that contains the type of information you want to filter, and then define how the filter returns the data. Each filter ideally contains more than one value, including the operator. Each filter can have up to 10 values maximum, and each dashboard can have up to three filters.

### Hands-on exercise for adding a filter to a dashboard

Let's use the **Account Dashboard** created in Chapter 9, *Building Dashboards in Lightning Experience* for this exercise; we will add a filter based on **Industry** (a field in **Account**) with the option equal to:

- **Apparel** or **Education**
- **Energy**
- Not blank

This allows users to analyze the numbers of accounts related to the **Account Industry**:

1. Open and edit the **Account Dashboard** dashboard. If you come from the **Dashboards** tab, you can click on the **Edit** link directly; it is located under the arrow action menu, as shown in the following screenshot:
   ![](img/72d01bea5a9ccf02f9452aab7505d9de_293_0.png)

2. At this moment, we have two components in this dashboard; let's add another component, say, `Account by Type`:
   ![](img/72d01bea5a9ccf02f9452aab7505d9de_294_0.png)

3. Create a new report `Account by Type`:
    - Click on the **Reports** tab
    - Click on the **New Report** button
    - Select the **Accounts** record type and then click on the **Create** button
    - Change the filter to **Show as All accounts** and **Date Field Range** as **All time**
    - Change the report format to **Summary**
    - Drag **Type** into the grouping area
    - Save the report with the name as `Account by Type` and Report Folder to **Unfiled Public Reports**

4. Go back to the dashboard from the Dashboard Builder and click on the + **Component** button.

5. Select the report just created, **Account by Type**.

6. Click on the **Add** button. This is shown in the following screenshot:
   ![](img/72d01bea5a9ccf02f9452aab7505d9de_295_0.png)

7. Move the new component added to the right of the second component.

8. Click on the + Filter button to add a filter.

9. Select the field you would like used as a filter; in this exercise, select Industry.

10. In Display Name, type Account Industry.

11. Click on the Add Filter Value button.

12. In the filter options, select **equals** in **Operator**. For **Value(s)**, select **Apparel** and **Education** and in **Display Text**, enter `Apparel or Education`. Consider the following screenshot:
    ![](img/72d01bea5a9ccf02f9452aab7505d9de_296_0.png)

13. Click on the **Apply** button for the first value and click on the **Add Filter Value** button again to add the second filter value.

14. In the filter options, select **equals** in **Operator**. For **Value(s)**, select **Energy**, in **Display Text**, enter `Energy`.

15. Click on the **Apply** button for the second value, and click on the **Add Filter Value** button again to add the third filter value.

16. In the filter options, select **not equal to**. For **Value(s)**, leave it blank; in **Display Text**, enter blank.

17. Click on the **Apply** button for the third value and then click on the **Add** button. This is what it would look like:
    ![](img/72d01bea5a9ccf02f9452aab7505d9de_297_0.png)

18. Back to the Dashboard Builder, click on the Save button and then click on Done to exit the Dashboard Builder.

19. Now we have a filter called Account Industry in the dashboard with three values defined. Consider the following screenshot:
    ![](img/72d01bea5a9ccf02f9452aab7505d9de_298_0.png)

21. To remove or edit the filter, go to the Dashboard Builder by clicking on the **Edit** button in the dashboard; next, look for the pencil icon (**Edit Filter**), as shown in the following screenshot:
    ![](img/72d01bea5a9ccf02f9452aab7505d9de_300_0.png)

From here, you can add a filter by clicking on the **Add Filter Value** button or delete the value by clicking on the cross icon. To edit the existing values, click on the filter value name; for this example, let's click on **Energy** and then select **Electronics** and change the Display Text to `Electronics or Energy`. Click on the **Apply** button and then click on the **Update** button. Click on the **Save** button and then click on **Done** to exit the Dashboard Builder. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_301_0.png)

You can have up to three filters in a dashboard. To add more filters, click on the Add Filter button in the dashboard edit mode.

### Viewing a dashboard

In terms of runtime, as we discussed in Chapter 9, Building Dashboards in Lightning Experience, when you open a dashboard, it will show the data based on the latest refresh (manual or scheduled); while for a report, when you open a report, it always shows the current data.
