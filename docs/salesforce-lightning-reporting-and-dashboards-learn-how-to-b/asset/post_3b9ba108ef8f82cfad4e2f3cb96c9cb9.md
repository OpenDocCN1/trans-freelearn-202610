
The preceding screenshot shows that **Amount** is summarized with **Sum**, while **Expected Revenue** is summarized with **Sum** and **Average**; this is valid for all formats of reports.

## Filters

To hide the filters panel on the right-hand side, just click on the arrow in the top-right section of the panel or click on the filters icon; toggling it again will bring back the filters panel.

In Lightning, all users with the permission to run the report will be able to change the default filter, such as **Show Me**, date range, and all additional filters not locked, even users who do not have permission to create and customize reports.

Let's discuss this in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_239_0.png)

This report **Opportunity by Stage** is an **Opportunity** report type. For a report with report type as **Opportunity**, you will see an additional default filter, **Role Hierarchy**, while other report types will not have it, they will only have filters for **Show Me** and **Date**. The user is able to change the filter when running the report. There is no need to edit the report; just click on the filter name to modify the filter values.

The following default filters would be the additional filters added by the report creator; the creator can leave it for the user to change the filter values or lock the filter. The Type field in the preceding report is not locked, so the running user is able to change or remove it from the filter, click on the filter to edit, or cross the icon in that filter to remove the filter. The following screenshot shows all filters added to the report, including Locked Filters:

![](img/72d01bea5a9ccf02f9452aab7505d9de_240_0.png)

The next filter Amount is locked, so the user will be not able to delete it. All the changes here will not be saved and will not affect other users unless the user has an edit permission and clicks on Save from the action button.

## The Collaborate field

If **Chatter Feed Tracking for Report** is enabled, this icon will be visible to all users able to open the report. To enable this, navigate to **Setup | Feature Settings | Feed Tracking**, look for **Report** in the left-side panel and tick **Enable Feed Tracking.**

By default, it will have **Post** and **Poll** for collaboration. The same is the case with all other **Chatter** posts; you can mention other users using @ followed by the username. Other users can **like**, **comment**, and **bookmark** the post, while for the user who posts in **Chatter** feed, the user also will be able to **Delete** and **Edit** the post.

## Report settings

For the **Matrix** and **Summary** reports, the user will have options to show or hide--**Details**, **Subtotals**, **Grand total**, and **Record counts**. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_241_0.png)

For **Tabular** reports, options are limited to showing or hiding **Details** and **Grand total**. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_242_0.png)

When the **Record Count** is hidden in **Summary** and **Matrix** reports, it will be hidden at the top as well. Hiding **Grand total** will hide the values from the bottom of the report as well as at the top, below the title. **Subtotals** are only available for the **Summary** and **Matrix** reports, which totally makes sense because there are no subtotals in the **Tabular** report.

## The action button

These are complete actions you can perform in an open report. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_242_1.png)

Depending on the user permissions, the user will see different actions; let's quickly explain each of the action available:

- **Edit**: This is used to edit the report in the report builder
- **Clone**: This is used to clone to a new report; you will have the option to store it as a private report, make it public, or store it to a specific folder
- **Save**: This saves the report with any changes made, such as adding a chart
- **Subscribe**: This subscribes a report for email delivery on a schedule; we will discuss this in the next section
- **Export**: This is used to export a report into comma-delimited `.csv` or Excel format `.xls`
- **Delete**: The report deleted will be available in the Recycle Bin for 15 days and can be restored at that time

## Subscribing reports

In Lightning Experience, you can subscribe to up to five reports and receive refreshed report results by email on a schedule that you set--Daily, Weekly, or Monthly.

To subscribe to reports, you need to have the **Subscribe to Reports** permission granted from **Profile** or **Permission Set**. On top of that, there are two more additional permissions related to this:

- **Subscribe to Reports: Add Recipients**
- **Subscribe to Reports: Set Running User**

These permissions require the **Subscribe to Reports** permission to be enabled.

This is a sample of an email sent to you when you subscribe for a report; you can click on the report title to open the report directly in Salesforce or click on the record name, such as Account Name or Opportunity Name, to open the report in Salesforce:

# Opportunity by Stage

As of 5/8/17 at 11:00 PM · Viewing as Johan Yu · Report subscription started by Johan Yu

All opportunities Close Date: Custom Amount greater than "10,000" Type equals Existing Customer - Upgrade,Existing Customer - Replacement,Existing Customer - Downgrade

Total Records Total Amount
14 $1,752,000.00

| Stage | Opportunity Name | Account Name | Amount Sum | Expected Revenue |
| Qualification (2 records) | United Oil Emergency Generators | United Oil & Gas Corp. | $30,000.00 | $3,000.00 |
|  | United Oil Refinery Generators | United Oil & Gas Corp. | $27,000.00 | $2,700.00 |
|  |  |  | $57,000.00 |  |
| Id. Decision Makers (1 record) | Edge Emergency Generator | Edge Communications | $35,000.00 | $21,000.00 |
|  |  |  | $35,000.00 |  |
| Negotiation/Review (2 records) | United Oil Installations | United Oil & Gas Corp. | $70,000.00 | $63,000.00 |
|  | United Oil Office Portable Generators | United Oil & Gas Corp. | $125,000.00 | $112,500.00 |
|  |  |  | $195,000.00 |  |
| Closed Won (9 records) | Edge SLA | Edge Communications | $40,000.00 | $40,000.00 |
|  | United Oil Installations | United Oil & Gas Corp. | $50,000.00 | $50,000.00 |
|  | United Oil Installations | United Oil & Gas Corp. | $235,000.00 | $235,000.00 |
|  | United Oil Plant Standby Generators | United Oil & Gas Corp. | $675,000.00 | $675,000.00 |
|  | United Oil SLA | United Oil & Gas Corp. | $120,000.00 | $120,000.00 |
|  | United Oil Standby Generators | United Oil & Gas Corp. | $120,000.00 | $120,000.00 |
|  | Edge Installation | Edge Communications | $50,000.00 | $50,000.00 |
|  | Project Motor Links in Kentica | Burlington Textiles Corp of America | $75,000.00 | $75,000.00 |
|  | Project Motor Head | Burlington Textiles Corp of America | $100,000.00 | $100,000.00 |
|  |  |  | $1,465,000.00 |  |
| Grand Total (14 records) |  |  | $1,752,000.00 |  |

6. You and Alvin Lee will receive the email with a report every Monday morning. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_246_0.png)

## Building Dashboards in Lightning Experience

Dashboards offer data visualization at a glance. The audience would be able to see the summary of the data in a graphical layout, including charts, matrix, and tables.

In Salesforce, a dashboard is one of the most interesting features. It consists of many components. It allows you to present many components in a wide blank canvas, so your audience can compare each of the components easily; for example, sales between the western region versus the eastern region, comparing last month sales with sales 2 months ago, and so on.

In this chapter, we will discuss the Lightning dashboard in detail, and you will be introduced to the **Dashboards** tab, with a navigation around the dashboard view, the dashboard builder, dashboard components, and the data source to support each component.

The way a dashboard works is not the same as a report. Reports show the current data, while dashboards show a snapshot of the data on which it was last refreshed.

By end of this chapter, you will be fully equipped with working knowledge of the Salesforce dashboard in Lightning, from creating a dashboard to scheduling dashboard refresh; this includes the schedule to deliver the dashboard refreshed by email.
