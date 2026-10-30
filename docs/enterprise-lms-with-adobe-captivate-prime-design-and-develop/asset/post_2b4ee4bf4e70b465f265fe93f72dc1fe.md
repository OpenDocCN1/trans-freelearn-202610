
These default reports already contain a lot of useful information. But that may not be exactly the data your organization is looking for. That is why, in addition to these default reports, you can also create your own custom reports and arrange them in your own custom dashboards. This is what we will review in the next section.

## Creating Custom Reports and Dashboards

The default reports discussed in the previous section provide administrators with a solid overview of what's going on within their Prime account. Most administrators in most organizations are interested in the metrics provided by the default reports. In some cases, these reports provide exactly the type of information you are looking for. But more often than not, organizations define goals and outcomes that are unique to them. So, they look for metrics and reports that help them document the specific outcomes they are seeking.

This is the reason why Captivate Prime lets you define a wide number of custom reports each allowing a wealth of variations and nuances. The possibilities are virtually endless, so every organization should find a way to extract from Captivate Prime the exact data they are looking for.

Just like the default reports, custom reports are also arranged into dashboards. By default, Captivate Prime comes with an empty default dashboard where you can arrange your custom reports as you see fit. But you can also create additional custom dashboards and share them with other users. These are the features that we will explore in this section.

To access the custom reports, it is necessary to take the following steps:

- 1. Log into your Prime account as an administrator.
- 2. From the administrator home page, click the **Reports** link located in the **Manage** section of the left sidebar.
- 3. Finally, click the **Custom Reports** link in the left sidebar to open the custom reports section of the administrator application.

When the page opens, notice the four tabs available at the top of the main content section, as illustrated in the following screenshot:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_379_0.png)

Figure 10.10 – The tabs of the custom reports page

We will review these four tabs one by one in the following few sections.

- 4. Make sure you are on the **Sample Reports** tab as illustrated in the preceding screenshot.
- 5. Take some time to examine the three sample reports provided.

The purpose of these three sample reports is to give you a sense of the kind of reports that you can define in Captivate Prime. They are the same in every Prime account and do *not* use the data available in *your* account. These three sample reports are arranged in a single **Sample Dashboard**. You will now use them to examine the basic capabilities of custom reports and custom dashboards.

- 6. Hover over the bars in the different reports to reveal the various tooltips available on a Prime report.
- 7. Click the – icon located in the upper-right corner of each report to collapse the corresponding report.
- 8. Click the + icon to expand the collapsed reports.
- 9. You can also move the reports around to rearrange them within the sample dashboard.
- 10. Scroll down the page until you see the **Skills vs Managers** report. Notice that this report spans the entire second row of the sample dashboard.

Because this particular report contains a lot of data, only part of the data is visible in the main area of the report.

- 11. Use the bottom area of the **Skills vs Managers** report to select the data visible in the upper portion of the chart. To do so, move the gray area left and right and see how this affects the chart.

Now that you have a better idea of what custom reports look like and how they can be arranged within a custom dashboard, you will create a few of these custom reports in the next section.

### Creating custom reports

There are so many custom reports that can be created in Adobe Captivate Prime that it is impossible to cover them all in this book. So, to prepare for this section, we contacted some Prime customers and partners and asked them what their top three custom reports are. The three reports that received the most votes are the following:

- The number of courses followed per profile
- The time spent learning per user
- The course effectiveness, by course

In this section, we will explore the options available for building custom reports by recreating these three reports one by one.

**The right Prime account**
While doing so, keep in mind that you are using a trial account with very few users and courses available, so there is very little data to feed these reports.
If you have access to an older Prime account with more users and courses, your custom reports will have a lot more data to display and you'll be able to feel more of their awesome power. So, feel free to use such an account for the exercises of this section if you have one available.

Right out of the box, Captivate Prime contains a single dashboard, which is empty by default. Use the following steps to access this default dashboard:

- 1. Make sure you are logged in as an administrator to your Prime account.
- 2. From the administrator home page, click the **Reports** link located in the **Manage** section of the left sidebar.
- 3. Next, click the **Custom Reports** link in the left sidebar.
- 4. Finally, open the **Dashboard Reports** tab, which is the second of four tabs available at the top of the page.

This opens the default dashboard available in Captivate Prime. As shown in the following screenshot, this dashboard is empty by default.

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_381_0.png)

Figure 10.11 – The default dashboard is empty by default

If you do not create a custom dashboard, all the custom reports you build in Captivate Prime are added to this default dashboard.

In the next section, you will build your first custom report and add it to this dashboard. You will learn how to create custom dashboards and move reports from one dashboard to another later in this chapter.

### The number of courses followed per profile report

The first report you will build will allow you to examine the number of courses followed by each profile. As discussed in Chapter 5, Managing Users, the profile is one of the fields available when creating new users in Captivate Prime. Depending on how your organization is structured, the profile of an individual may have different meanings, but most of the time, the profile of a person corresponds to the job title or the department in which the person is working.

This report will allow you to determine how the learning activities are distributed across profiles and to answer questions such as the following:

- Are all the profiles learning in the same way?
- Are there profiles that follow more courses than others? Does that translate to these profiles performing better?
- What are the profiles that learn less than others? How does that affect their performance? How can we explain that some profiles learn more, and others learn less?

Follow these steps to build this first custom report:

- 1. Make sure you are on the empty default dashboard page as discussed in the previous section.
- 2. Click the Add Report link located in the middle of the default dashboard. You can also click the Add button located in the upper-right corner of the page.

This action opens the Add Report dialog.

- 3. Open the **Type** drop-down menu located in the upper portion of the **Add Report** dialog.
- 4. Examine the available options, then choose the **Courses Enrolled and Completed** option.

Choosing a report **Type** as you just did gives default values to most of the other options available in the **Add Report** dialog. These report types can therefore be seen as *templates* to help you build custom reports quickly. In this example, choosing the **Courses Enrolled and Completed** report type has automatically adjusted the following properties:

- In the **Report** section, a default name has been added in the **Name** field.
- In the **Show Report for** section, the **Y-axis** dropdown is set to **Course Statistics** and the **For (X-axis)** dropdown is set to **Course**. Remember that the *y* axis of a chart is the *vertical* axis, while the *x* axis is the *horizontal* axis. Also notice that the *y* and *x* axes are both mandatory.
- The **And (Secondary Y-axis)** dropdown is currently set to **None**. Notice that this option is not mandatory.

These settings take you pretty close to the report you want to build. You will now adjust some of these parameters, so you get exactly the right report.

- 5. Open the **For (X-axis)** drop-down menu and examine the available options. When ready, choose the **Profile** option.
- 6. Open the **Time Span** section and confirm that the **Date** dropdown is currently set to **YTD** (year to date). Take some time to examine the other options available, but make sure you do not make any change.
- 7. Open the **Filters** section. Notice that you can filter the report by **Manager**, **Profile**, **User Group**, and **Content**. By default, there are no filters defined. For this first example, make sure you do not add any filters.
