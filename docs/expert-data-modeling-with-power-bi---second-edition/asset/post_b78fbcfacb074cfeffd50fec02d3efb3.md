
![](img/5d2bce74fd6fceebed14fd1488bf7088_195_0.png)

Figure 4.16: Yearly Product Category Sales data stored in Excel

The aim is to load the preceding pivot table into Power BI as follows:

- 1. In the Power Query Editor, click the New Source drop-down button.
- 2. Click Excel.
- 3. Navigate to the folder containing the Excel file and select the Excel file.
- 4. Click Open.
- 5. Select the Yearly Product Category Sales.xls sheet.
- 6. Click OK.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_197_0.png)

Figure 4.18: Transforming a pivot table

Follow these next steps to fix the preceding issues one by one:

- 7. Remove the Change Type step.
- 8. Click the Column8 column.
- 9. Click the Remove Columns button from the Home tab, as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_197_1.png)

Figure 4.19: The excessive step and column removed

We remove the Grand Total row with the following steps:

- 10. Click the Remove Rows button from the Home tab and click Remove Bottom Rows.
- 11. Type 1 in the Number of rows textbox.
- 12. Click OK.

![](img/5d2bce74fd6fceebed14fd1488bf7088_200_0.png)

## Figure 4.23: Removing unnecessary steps and unpivoting columns

The very last steps are listed here:

- 19. Rename the Attribute column to Year and rename Value to Sales.
- 20. Change all column types by selecting all columns.
- 21. Click the Detect Data Type button from the Transform tab, as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_200_1.png)

## Figure 4.24: Renaming the columns and changing the columns’ types

Figure 4.25: Opening the Excel file from SharePoint Online in Excel Desktop App

This opens the Excel file in the Excel Desktop application. In Excel, follow these steps:

- 4. Click the File menu
- 5. Click Info
- 6. Click the Copy path button

The following image shows the preceding steps:

Figure 4.26: Copying Excel path from Excel Desktop App

So far, we have got the Excel file path. The next step is to get data from the copied path in Power BI Desktop.

Open Power BI Desktop and follow these steps:

- 7. Click Get data
- 8. Click Web:

![](img/5d2bce74fd6fceebed14fd1488bf7088_203_0.png)

Figure 4.27: Getting data from Excel stored in SharePoint Online in Power BI Desktop

- 9. Paste the path we copied from Excel into the URL textbox
- 10. Delete the ?web=1 part from the end of the copied path
- 11. Click OK

The following image shows the preceding steps:

![](img/5d2bce74fd6fceebed14fd1488bf7088_203_1.png)

Figure 4.28: Modifying the path copied from Excel to get the data in Power BI Desktop

We get the following error if we miss step 10, as shown in the following image:

Details: “The input URL is invalid. Please provide a URL to the file path on SharePoint up to the file name only (with no query or fragment part).”

![](img/5d2bce74fd6fceebed14fd1488bf7088_204_0.png)

Figure 4.29: An error is raised if we forget to take out the query part from the file path

- 12. From the access web content page, click Organizational account
- 13. Click Sign in and pass your credentials
- 14. Click Connect

The following image shows the preceding steps:

## Method 1: Getting the excel file path directly from SharePoint

In SharePoint Online, we can get the file path directly from the file details, so this method works for all types of files such as CSV, Excel, and so on. The following steps show how to get the file path in SharePoint Online:

- 1. Select the Excel file
- 2. Click the ellipsis button
- 3. Click Details
- 4. In the Details pane, scroll down to find the Path section
- 5. Click the Copy Direct Link button

The following image shows the preceding steps:

![](img/5d2bce74fd6fceebed14fd1488bf7088_206_0.png)

Figure 4.32: Copying a direct file link from File Details in SharePoint Online

In Power BI Desktop, just click Get data, then select Web, and paste the link in the URL textbox as the following image shows:

![](img/5d2bce74fd6fceebed14fd1488bf7088_207_0.png)

Figure 4.33: Getting data from Excel hosted in SharePoint Online with a direct link

The following list shows the benefits of this method over the first method:

- It requires fewer clicks to get the direct link
- The direct link does not have any query parts, so we do not need to modify the link
- The whole process is easier to remember

The next section explains connecting to Power BI datasets.

## Power BI datasets

Considering the increasing demand for team working and collaboration, using Power BI datasets is becoming more and more popular. When we create a data model in Power BI Desktop and publish it to a Power BI Service workspace, the underlying transformation pipelines in Power Query and the data model turn into a dataset. Then, we make the datasets available across the organization. This is an efficient method of collaboration in large Power BI projects. The data modelers create the models and publish them to the service, and the report writers make thin reports on top of the available shared datasets.

Note
Shared Power BI datasets are not available in the Power BI free licensing plan. The reason is that accessing Workspaces requires a Pro license;

![](img/5d2bce74fd6fceebed14fd1488bf7088_211_0.png)

Figure 4.37: The underlying data model of a connected Power BI dataset

We can create new layouts on top of the model, as shown in the following image:

## Figure 4.38: Creating new layouts in the data model

Note that the icon is used for the tables in Connect Live.

There is one more point to note: we can only see and access the datasets from workspaces that have Admin, Member, or Contributor permissions.

The other method to connect to a dataset is to turn the connection into DirectQuery mode. This has been made possible by the DirectQuery for PBI datasets and AS feature currently in public preview. Follow these steps to enable it:

- 1. Click File
- 2. Click Options and settings
- 3. Click Options
- 4. From the GLOBAL section, click Preview features
- 5. Tick the DirectQuery for PBI datasets and AS feature
- 6. Click OK

The following image shows the preceding steps:

![](img/5d2bce74fd6fceebed14fd1488bf7088_213_0.png)

Figure 4.39: Enabling the DirectQuery for PBI datasets and AS preview feature

Now that we have enabled the DirectQuery for PBI datasets and AS preview feature, we have various options to connect to a dataset in DirectQuery mode. We have to first connect to the dataset in Connect Live mode, then turn Connect Live to DirectQuery as follows:

- 1. Click the Make changes to this model button from the Modeling tab on the ribbon.
- 2. Click the Make changes to this model link from the status bar.
- 3. From the Model view, click the Make changes to this model button from the Home tab on the ribbon.
- 4. After performing the preceding steps, a warning message appears; click the Add a local model button.

![](img/5d2bce74fd6fceebed14fd1488bf7088_215_0.png)

## Figure 4.41: Selecting tables to be included in the DirectQuery mode

We can modify the data model after switching the connection to DirectQuery. We can also get data from various sources to create a Composite mode. We discuss this in more detail in Chapter 14, DirectQuery Connections to Power BI Datasets and Analysis Services in Composite Models.

The model looks like the following image after switching to DirectQuery:

connect to an existing dataflow:

- 1. Click the Get data dropdown.
- 2. Click Power BI dataflows.
- 3. A list of all dataflows available to you shows up in the Navigator window. Expand the desired workspace.
- 4. Expand the dataflow model.
- 5. Select a table.
- 6. Click Load.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_217_0.png)

Figure 4.43: Getting data from Power BI dataflows

At this point, if we meet the requirements, Power BI prompts us to select the storage mode from either Import or DirectQuery mode, as the following image shows:
