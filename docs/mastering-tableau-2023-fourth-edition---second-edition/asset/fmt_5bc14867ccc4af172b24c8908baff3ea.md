
Revision histories are so helpful! I hope you will not have to use them often but it’s always good to have options. The next question many organizations have is, how can I make sure that users only see what they are supposed to see and not just everything that has been published to Tableau Server? One solution is user filters. We will discuss this topic next!

## User filters

Students often ask me something along the lines of the following question: I have sales managers over various territories. It’s important that the sales managers see only their metrics; that is, not the metrics of the other sales managers. In order to accomplish this, do I have to create separate workbooks for each sales manager?

Fortunately, the answer is no. Tableau provides user filters that allow the Tableau author to make sure that each of those sales managers sees only the information for which they have clearance. To demonstrate this, take the following steps:

1. This exercise needs more than one user on Tableau Server. Therefore, go ahead and create at least one additional user on Tableau Server by clicking on `Users | Add Users`. I used a second email address that I own to do so:
   ![](Images/B18435_14_16.png)
   Figure 14.16: Add Users
2. As an optional step, you can change an account's display name. To change the display name of an account, log in to [https://www.tableau.com/](https://www.tableau.com/), click on `User | Edit Account`, and make the desired changes to the first and last name entries.
3. As a second optional step, instead of assigning single users to a filter, you can also assign groups of users. To do so, you can create different groups, as follows:
   ![](Images/B18435_14_17.png)
   Figure 14.17: Add Group
4. You can then split the users on your Tableau Server instance into `Central`, `East`, `South`, and `West` regions by selecting a user and then `Group Membership…` under `Actions`:
   ![](Images/B18435_14_18.png)
   Figure 14.18: Add Group II
5. Now, access the workbook associated with this chapter and navigate to the `View Level User Filter` worksheet. Note that the view is a field map of the USA with `State` on the `Detail` shelf, `Region` on the `Color` shelf, and `Sales` on the `Label` shelf.
6. Log in to an instance of Tableau Server via `Server | Sign In from Tableau Desktop`.
7. Select `Server | Create User Filter | Region...` (only accessible from Tableau Desktop):
   ![](Images/B18435_14_19.png)
   Figure 14.19: Create User Filter
8. In the resulting `User Filter` dialog box, you can now assign either a single user or a group to a region. Name the user filter `Sales Region` and click on `OK`:
   ![](Images/B18435_14_20.png)
   Figure 14.20: User Filter
9. Note that `Sales Region` is now actually added to the `Sets` portion of the `Data` pane:
   ![](Images/B18435_14_21.png)
   Figure 14.21: Set added
10. Place the `Sales Region` on the `Filters` shelf.
11. In the right portion of the status bar, click on the dropdown to access the user filter:
    ![](Images/B18435_14_22.png)
    Figure 14.22: Switching between filters
12. Choose a different group or user and note that the results displayed change depending on the settings from Figure 14.20.

Once the workbook is published to Tableau Server, users who access the workbook will only see the information the filter allows. This can also be applied to saved data sources at the data source level.

One important thing to note is, as opposed to simply adding a dimension filter to the `Filters` shelf, you might want to consider adding the `Sales Region` to the `Data Source` filter. This is important because any user with web authoring permission can simply remove a filter from the `Filters` shelf. In the case of this exercise, the user would then be able to see metrics for every region. Data source filters, however, cannot be removed via the web authoring environment and are thus more secure. Furthermore, a data source filter is workbook-wide, which further secures the data. However, if you don't have web authoring enabled, the `Filters` shelf serves its purpose.

For more details on filters and Tableau's order of operation, please check *Chapter 13*, *Improving Performance*.

# More Tableau Server settings and features

Once a dashboard has been published, users can set alerts, certify data sources, subscribe, add comments to a dashboard, and more, depending on their permissions. This section will address the different functionalities in two different places: on a worksheet and on a view. Next to those, Tableau Server has many more functionalities, especially settings for site admins, which are—unfortunately—out of the scope of this book.

## Features on the worksheet level

By *features on the worksheet level*, I mean the settings that are available to you after selecting a project and a certain worksheet like so:
![](Images/B18435_14_23.png)
Figure 14.23: Tableau Server

On this level, we see eight different tabs, which we will discuss over the next few pages.

### Views

Views show a thumbnail of all the worksheets or dashboards that have been published within one workbook. When uploading a workbook to Tableau Server, you can select all the views that should show here:
![](Images/B18435_14_24.png)
Figure 14.24: Publish Workbook

By clicking on the three dots next to a View, more options will show, as can be seen in the following screenshot:
![](Images/B18435_14_25.png)
Figure 14.25: Options

The different options are self-explanatory: you can edit the view (also referred to as web authoring, which we've discussed earlier in the chapter), and you can share the view with other users, where Tableau will send an email with an image and a hyperlink to the user you shared the view with. You can also add tags that will help other users find the view, you can check which user or group has permission to open the view and who has seen this view, and lastly, you can delete it. Which of those options will display is a matter of the configuration in Tableau Server, so your admin can enable or disable the different features to show or else they might be grayed out.

### Data Sources

The next tab is **Data Sources**. It will give you an overview of the data sources used to create this workbook:
![](Images/B18435_14_26.png)
Figure 14.26: Data Sources

You will be able to see which type of data sources were used; for example, with an extract of live connections, by clicking on the three dots by each source, you will again be able to see more settings, depending on the settings of your Tableau Server:
![](Images/B18435_14_27.png)
Figure 14.27: Data Sources II

### Connected Metrics

Next in line are the connected metrics. This is a relatively new feature. To see metrics, you first have to set them up in a view. We will get to this in the second part of this section, but basically, it allows you to select pieces of different dashboards and views and combine those data points as metrics to monitor your data. Metrics make use of a specific data mark in your view. Therefore, you might need to adjust your view to show the one mark you want to use.

For example, if you have a dashboard with daily profits over the past year but your metrics should show only the last profit data point, you will have to create a new view in order to use the explicit last profit in a metric, even though the data is already part of the initial view. However, a big advantage is that you don't need to connect to different data sources, as the connections that are already set up in the different workbooks will be leveraged.

More details regarding metrics can be found here: [https://www.tableau.com/about/blog/2020/5/introducing-metrics-headlines-your-data](https://www.tableau.com/about/blog/2020/5/introducing-metrics-headlines-your-data).
