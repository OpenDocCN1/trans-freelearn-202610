
Throughout this chapter, we will provide notes and tips to help you to understand important items. The following topics will be covered:

- Opening and searching a dashboard
- Navigating a dashboard
- The dashboard builder
- Dashboard refresh
- Dashboard components

# Introducing dashboards in Salesforce

A dashboard is visualization data that displays the key metrics based on the criteria in reports, a running user, and with the option of dashboard filters. A powerful dashboard consumes multiple reports that often have a common theme, such as global sales, customer support, and so on. Dashboards visualize data stored in Salesforce and help you identify trends, sort out quantities, and measure the impact of their activities.

# Built-on components

A dashboard is usually built on many dashboard components. We can mix types of charts or tables of dashboard components in a dashboard. Each dashboard component always needs to have one report as the data source. However, we can use the same report as the data source for multiple components within the same dashboard (for example, use the same report in both a bar chart and pie chart), or we can use it in different dashboards.

# Dashboard accessibility

Similar to reports, dashboards are also stored in folders, but they are different types of folders. The folder controls who can access the dashboards, not the dashboard itself. If you have access to the dashboard folder, you can view all the dashboards in that folder. As dashboards are built on components and each component needs a report served as the data source, so the dashboard running user needs to have access to the report too.

## Running user

Different from reports, each dashboard has a running user defined, who will use the security settings in determining the data to display in the dashboard. If the running user is a specific user, all dashboard viewers will see the dashboard data based on that user, including data security and hierarchy. For this reason, when you create a report, you need to choose the right running user for the dashboard to show the correct data. For example, set the **Country Manager** as the running user for a country sales performance dashboard; this will allow all users to see the overall country sales performance. When you build a performance leaderboard for the team, all users will see the same data, so make sure to set the dashboard viewer as the **Country Manager**.

## Dynamic dashboards

Dynamic dashboards are totally different from the standard dashboards explained earlier. With dynamic dashboards, data shown in the dashboard will follow the running user visibility, which is always the logged-in user. This way, each user sees the dashboard according to his or her own access level. We'll cover dynamic dashboards in detail in *Chapter 10, Learning Advanced Dashboard Configuration.*

Users with specific permission will be able to change the running user when they run the dashboard; this is only if the dynamic dashboard is configured to allow that. This is very useful for the top management in analyzing data by changing the running user, but there is no need to change the dashboard or reports filter.

# The new experience

If you switch from Classic, Salesforce has to redesign the whole dashboards experience in Lightning, but existing dashboards built in Classic will work in Lightning without the need to migrate or reconfigure. Lightning Experience introduces a new dashboard builder and offers the flexibility to create components of different sizes:

- **Flexible dashboard components size:** You can drag the corners and sides of the dashboard components to resize them. The components can have a size from one column and one row to as big as you want. The charts and all other items, such as legends, will be automatically resized when you resize the component size. You can configure the chart starting from chart type, chart title, chart axis, and others, by clicking on the pencil icon. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_251_0.png)

- **Drag and drop dashboard components:** You can arrange the dashboard components' position by dragging and dropping the components. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_252_0.png)

- **Dashboards with more than three columns:** In Lightning Experience, you can have a dashboard with up to nine columns, whereas in Classic, we only have a maximum of three columns.

# Opening a dashboard

If you have the **Run Report** permission, you will be able to view a dashboard that you have access to, from the **Dashboards** tab, from the search result, or from the favorite icon. Similar to reports, dashboard accessibility is controlled by the folder that stores that dashboard.

From the dashboards tab, if you have the Create and Customize Dashboards permission, you will be able to create a new dashboard by clicking on the New Dashboard button, or to edit existing dashboards stored in folders that you have edit access. Check Chapter 2, Concepts and Permissions in Reports and Dashboards and Chapter 3, Implementing Security in Reports and Dashboards for a complete reference on the permissions for dashboards and in relation with the dashboard folder that is used to store the dashboard.

Let's start with opening a dashboard in Salesforce Lightning; remember that dashboards are stored in the dashboard folder, either in the private or public folder. Private reports are only accessible by users who save the dashboard in the Private Dashboards folder; we have discussed this in depth in Chapter 2, Concepts and Permissions in Reports and Dashboards, so we will not repeat it here.

There are multiple ways to open a dashboard in Salesforce Lightning. First, click on the Dashboards tab.

# The Dashboards tab

Here is a screenshot taken after clicking on the Dashboards tab; let's go through all the components here:

![](img/72d01bea5a9ccf02f9452aab7505d9de_253_0.png)

1. **Title (denoted by 1):** This will show the menu selected, the number of items for that folder, and information if the list of dashboards is sorted with a column (if any).
2. **Recent (2):** When you click on the **Dashboards** tab, it will show a list of the dashboards you recently opened by default.
3. **The DASHBOARDS menu (3):** There are four menus in the dashboards area.
4. **The FOLDERS menu (4):** There are three menus in the folders area.
5. **The New Dashboard button (5):** Click on this button to create a new dashboard; this is visible only when you have the permission to create and customize the dashboard.
6. **The New Folder button (6):** Click this button to create a new folder for the dashboard; this is visible only when you have the permission to create dashboard folders.
7. **Dashboards or folder area (7):** This is the list of dashboards or folders depending on the selected menu on the left-hand side.
8. **The action button (8):** From here, you will be able to view (the same as clicking on the dashboard name), edit, or delete a dashboard or delete the folder. Visibility of the menu depends on user permissions.
9. **Sort (9):** You can sort the dashboard or folder list in an ascending or descending order by clicking on the header.

Similar to the **Reports** tab or the other objects tab in Lightning, if you click on the arrow next to the **Dashboards** tab, it will show the dashboards that you have added to your favorites and the list of dashboards you recently opened. Adding a dashboard to favorites is similar to adding a report or other records to favorites; refer to Chapter 8, *Working with Reports*. The following screenshot shows the **Dashboards** tab menu when you click on the arrow icon next to the **Dashboards** tab, including **MY FAVORITES** and **RECENT RECORDS**:

![](img/72d01bea5a9ccf02f9452aab7505d9de_255_0.png)

# The DASHBOARDS menu

After you click on the **Dashboards** tab, you will see two main panels: the left-hand side panel and the right-hand side panel. The left-hand side panel contains categories of the **DASHBOARDS** and **FOLDERS** menus, and right-hand side panel will show a list of dashboards or folders related to the menu selected in the left-hand side panel. This is shown in the following screenshot:
