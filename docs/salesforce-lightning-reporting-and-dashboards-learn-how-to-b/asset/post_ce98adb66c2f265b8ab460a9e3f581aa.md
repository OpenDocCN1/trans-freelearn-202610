
From the preceding screenshot, there are five menus under **Reports**:

- **Recent**: This is the default selection after clicking on the **Reports** tab. When this is selected, the right-hand side panel will list all reports recently opened by that user, so my recent list will be different from my colleague's list.
- **Created by Me**: Clicking on this will show all reports created by the user, including reports stored in private and public folders.
- **Private Reports**: This includes all reports stored by the user into the **My Personal Custom Reports** folder.
- **Public Reports**: These are reports stored in the **Unfiled Public Reports** folder; all users with the run report permission will be able to run reports stored in this folder.
- **All Reports**: This will show all reports the user has access to, from both private and public reports. If your organization has too many reports that are no longer used, it is a good practice to delete them periodically, so the list will be not too big here.

When you select any menu here, the right-hand side panel will list all reports based on the menu selected on the left-hand side. There will be five columns and one action column in this panel: **REPORT NAME**, **DESCRIPTION**, **FOLDER**, **LAST MODIFIED BY**, **SUBSCRIBED**, and an arrow icon for the dropdown of actions. You will be able to sort the list by **REPORT NAME**, **FOLDER**, **LAST MODIFIED BY**, and **SUBSCRIBED**. The **SUBSCRIBED** column will be visible only when the user has a **Subscribe report** permission.

Let's see the action menu available for each report:

![](img/72d01bea5a9ccf02f9452aab7505d9de_229_0.png)

For the actions menu in the last column for each report, the menu visibility will depend on the user permission:

- **Run:** This is the same as clicking on the report name to run the report.
- **Edit:** This option will be visible only when the user is able to edit the report.
- **Subscribe:** This option will be visible if the user has the **Subscribe** report permission.
- **Export:** This option will visible if the user has an Export report permission.
- **Delete:** This option will be visible if the user is able to delete the report based on user accessibility in the report folder. The user will be able to delete all reports in their private folder.

## The Folders menu

As with the **Reports** category, under the **Folders** category, there are multiple menus which will filter the folders shown in the right-hand side panel. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_230_0.png)

There are three menus under **Folders**:

- **Created by Me**: This will list all the report folders that you create
- **Shared with Me**: This will list all the report folders created by someone else but shared with you
- **All Folders**: This will list all the report folders you have access to, either created by yourself or someone else

When you select any menu here, the right-hand side panel will list all the report folders based on the menu selected on the left-hand side. There will be four columns and one action column. The columns would be **FOLDER NAME**, **CREATED BY**, **LAST MODIFIED BY**, **CREATED ON**, and an arrow icon for the dropdown of action.

Click on the arrow in the last column in the folder row; this will show the **Delete** menu if the user has permission to delete the folder. For users with permission to delete the folder, the user will be able to delete the report folder from here, but only for folders without any reports in that folder; otherwise, the user will receive an error message.

You also can sort the list by any column by hovering your mouse over the header and clicking on the column header. Click once to sort by ascending order, and click again to sort by descending order.

## The favorites icon

In the top-right section of the screen, before **Global Actions** (the **+** icon), take a look at the star icon with an arrow next to it, that is for adding to favorites, while the arrow next to it is used to open the item that has been added to favorites. Favorites let you quickly access important records, lists, groups, dashboards, and other frequently used pages in Salesforce, including reports.

![](img/72d01bea5a9ccf02f9452aab7505d9de_231_0.png)

Favorites are similar to bookmarks in a web browser but better because your favorites are available no matter which device or browser you use to log into in Lightning Experience. So, each user will have their own list of favorites.

## Hands-on exercise for adding an item to favorites

Let's consider a use case: because you always deal with reports, specifically a report called **Opty by Stage (2016 H1)**, add the reports tab and the report as a **Favorite**.

You can add the **Reports** tab as a favorite item by following these steps:

- 1. Click on the **Reports** tab.
- 2. Click on the star icon.
- 3. The system will pop up with the "**Reports**" was added to your favorites message. Consider the following screenshot:
- 4. Once added, confirm that it has been added by clicking on the arrow next to the star in the favorite icon. Refer the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_232_0.png)

![](img/72d01bea5a9ccf02f9452aab7505d9de_232_1.png)

- 5. Now you should see **Reports** with the report icon under **MY FAVORITES**.
- 6. If you click on **Reports** from here, it is the same as you clicking on the **Reports** tab.
- 7. You can rename or delete it by clicking on the **Edit Favorites** link.

Instead of just adding the **Reports** tab as a favorite, you can add an individual report as a favorite too. Let's follow these steps:

- 1. Open an existing report. In my case, I will open a report called **Opty by Stage (2016 H1)**.
- 2. Click on the star icon.
- 3. The system will pop up with the "**Opty by Stage (2016 H1)**" was added to your **favorites** message.
- 4. Click on the arrow next to the star in the favorite icon, and you should see that **Opty by Stage (2016 H1)** is added to **MY FAVORITES** as shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_233_0.png)

Once **Reports** is added into favorites, it will be listed under **MY FAVORITES** of the **Reports** tab too. Instead of clicking on the **Reports** tab, click on the arrow next to the **Reports** tab to show reports in **MY FAVORITES** and **RECENT RECORDS** for recently opened reports. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_234_0.png)

This step can be applied to **Dashboards**, **Account**, or any other records and tabs. The items added to favorites will be shown as **MY FAVORITES** in the object tab too. This is shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_234_1.png)

But if you hit the *Enter* button or click on the magnifying glass, the system will show all results on the Search Results page as shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_236_0.png)

- 6. **Chart:** Click on this to add or hide or change a chart in a report. We discussed charts in depth in *Chapter 7, Adding Charts in Reports and Pages.*
- 7. **Filter:** Click on the icon to hide or show the filter panel, denoted by number 7 in the preceding screenshot.
- 8. **Collaborate:** This will be visible only if **Chatter Feed Tracking** for report is enabled, we will discuss **Chatter Feed Tracking** for collaboration in *Chapter 11, Advanced Tips and Tricks for Reports and Dashboards.*
- 9. **Refresh:** Click on the icon to refresh the data shown in the report, denoted by number 9 in the preceding screenshot.
- 10. **Settings:** To show/hide the report result, the options available here will depend on the report format.
- 11. **The action button:** If a user has more than one permission for the report open, there will be an arrow next to the button, and this button will include **Edit**, **Clone**, **Save**, **Subscribe**, **Export**, and **Delete**.

## Summarizing fields

When your report has summarized fields configured in a report and **GRAND TOTAL** (under **Settings**) is enabled, all summarized fields will be shown next to the report title:

![](img/72d01bea5a9ccf02f9452aab7505d9de_238_0.png)
