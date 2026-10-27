
## 2

## Concepts and Permissions in Reports and Dashboards

This chapter will explain the concept of reports and dashboards in Salesforce, and discuss the data source to generate reports and dashboards. We will continue with the mechanism on reports and dashboards stored in Salesforce with an explanation of the types of report and dashboard folders.

We will continue with permissions, which is a basic setting that controls what a user can do in Salesforce. The same is true for reports and dashboards; many permissions surface around reports and dashboards, such as the ability to create and modify reports, export reports, schedule reports, and so on.

By end of this chapter, you will have gained a strong understanding of data use for reports and dashboards, permissions around users, and granting permissions to a user with Profile and Permission Set.

Throughout the chapter, we will provide notes and tips to help you to understand important items. The topics covered in this chapter are as follows:

- Data for reports and dashboards
- Generating reports and dashboards
- Report and dashboard folder
- Report permissions
- Dashboard permissions

## Data for reports and dashboards

First things first. You can only generate a report for data that exists in your Salesforce organization, unless you purchase Salesforce Connect to connect to an external database, which maps data stored outside into your Salesforce organization; the external data source could also be another Salesforce organization. Your report quality depends on the data quality, cleanness, and completeness. Therefore, system adoption is one of the most important keys to have great data and reports in Salesforce.

Once a report is created, you can store it privately, share with other users, subscribe to notifications when data in the report is changed and meets the conditions set, or schedule it for delivery to yourself and to other users.

In Salesforce, a report is the basic requirement to build a dashboard. Each component in a dashboard needs to have a report to serve as the data source. You can use the same report for multiple components in a dashboard, or across multiple dashboards.

You can use any field (with any field type) available in an object, or from multiple objects to create a report. You need to have read access to the object and the field, but it is not necessary to have permission to edit the data. If you need to learn more about object, field and field type, check out the The Salesforce object model section of Chapter 1, Fundamentals of Salesforce Reports and Dashboards. As you see now, objects and fields are foundations in creating reports.

## Generating reports and dashboards

When you click on a report, Salesforce generates the report on the fly within seconds, and the data shown in the report is live data from your Salesforce organization. But what if I would like to see a report from yesterday's data or last week's data? No, you can generate report only for current data. If you would like to see historical data, you can set up Reporting Snapshot or Field History, but that's not the whole data snapshot on a certain date.

In the report, you can only see objects and fields that you have access to, assuming that the report is created by someone else. This means that for the same report, you may see fewer fields or columns than other users, because you do not have permission to those fields.

Similarly, for a data set, if the record is not visible to you in Salesforce, you will not see it in the report either. This is referred to as the object sharing setting and object permission.

However, dashboard is a different story. When you open a dashboard, it shows data as of the last dashboard refresh. If the dashboard was last refreshed by your colleague, you will see the data as of your colleague's last refresh, or you can refresh it manually. Salesforce offers the ability to auto refresh the dashboard, but within a limit.

## The report and dashboard folder

Where are the reports and dashboards stored in Salesforce? They are stored in **Folders**. There is no limit on the number of reports and dashboards you can create and store. Folders created for reports can only be used to store reports, and the folders created for dashboards only to store dashboards. For simplicity in this book, let's call them the report folder and dashboard folder.

You can think of a folder here is similar to the folder in Windows Explorer, but without the drive and without a subfolder, and instead of storing files, it is used to store reports or dashboards depending on the folder type. As a folder manager, you can share the folder visibility to certain users, roles, roles and subordinates, or public groups.

Types of folders in Salesforce for reports and dashboards:

- A private folder
- Unfiled Public Folder (only for report)
- Public folder

### A private folder

A private folder is a special folder available for each Salesforce user, and is also known as the **My Personal Custom Reports** folder for a report folder. For dashboards, it is called **My Personal Dashboards** (in Classic), or **Private Dashboards** (in Lightning Experience). In Lightning Experience, reports stored in this folder are called **Private Reports**.

### Unfiled public folder

The **Unfiled Public Reports** folder in Classic, also known as the **Public Reports** folder in Lightning Experience, is another special report folder. Reports stored in this report folder are accessible by all users. If you also have permission to create and customize reports, then you will be able to store the reports into this folder.

### Public folder

To create a public folder, you need to have the permission to create a report folder or a dashboard folder (we will discuss all permissions related to the folder in Chapter 3, *Implementing Security in Reports and Dashboards*). The public folder is the folder that you create for storing reports and dashboards; normally, this folder is shared by many users.

After you create a new report, you need to save the report to run it. You can store the report in a public folder to share with your colleagues for future usage or other purposes, for example, as the data source for a dashboard, or for e-mail delivery. Depending on the user permission and folder-level access, this will determine the folder's visibility and accessibility to a user--whether as read-only or with read-write accessibility. Remember, report accessibility is configured on the folder level, not in the report itself.

![](img/72d01bea5a9ccf02f9452aab7505d9de_50_0.png)

When a folder is visible to a user, the user will be able to see and run all the reports stored in that folder. So the control is not by report, but by folder.

When you install an app from AppExchange, some apps will come with reports, and will be stored in their own folder. The folder will be created when you install the app.

## Concepts and Permissions in Reports and Dashboards

Click on the Reports tab, then on All Folders to see all the report folders that you have access to. Let's compare users. The following screenshot is taken from an admin, which has access to all the folders:

![](img/72d01bea5a9ccf02f9452aab7505d9de_51_0.png)

Compare this with the preceding screenshot with the following one taken from a Sales user. This Sales user can see only one folder instead of three folders, as in the preceding screenshot. The report folder manager can set folder accessibility for each user:

![](img/72d01bea5a9ccf02f9452aab7505d9de_52_0.png)

Each public folder can store either reports or dashboards, but not both. It is a good practice to have a good naming pattern for report names and folder names, for example, NA Sales Reports for a folder to store all reports related to Sales for North America, and EMEA Sales Dashboards folder for all dashboards related to EMEA sales. This will help you and other users to find the report easily, and keep things organized.
