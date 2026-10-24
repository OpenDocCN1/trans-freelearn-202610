

## 12 Power Query

In a book about nuances and quirks in Excel, I have saved what I consider to be the most nuanced and quirky feature for last: Power Query. I can attest that I picked up and sat down with Power Query multiple times over a series of months before it finally clicked for me. This feature has been around for 10 years, yet most Excel users are unaware of its immense potential.

In this chapter, my goal is to help you use Power Query to automate repetitive processes without writing any programming code. I only have room to share a few examples with you, but along the way, I’ll point out a variety of obstacles and timesavers that will jumpstart your effectiveness. If you find this chapter whets your appetite, Power Query Cookbook, by Andrea Janicijevic, is a comprehensive resource that goes far beyond what I had space to share here.

In this chapter, we will cover the following topics:

- Introducing Power Query
- Creating a list of worksheets
- Automatic report cleanup
- Extracting data from PDF files
- Unpivoting data
- Appending and merging data from multiple sources
- Connecting to databases and installing ODBC drivers

## Technical requirements

You will have the best experience in Power Query if you’re using Microsoft 365 or Excel 2021, but most of what I will share can be conducted in Excel 2010 and later. At the time of writing, Power Query is in beta testing for Microsoft 365 users of Excel for macOS. Anyone using Excel 2010 or Excel 2013 will need to download and install the free Power Query add-in. You can access Power Query in Excel 2016 by way of Data | New Query instead of Data | Get Data, which you must do in Excel 2019 and later. Microsoft 365 or Excel 2021 is required to extract data from PDF files. The workbooks, PDF file, and text files that I used in this chapter can be downloaded from GitHub at https://github.com/PacktPublishing/Exploring-Microsoft-Excels-Hidden-Treasures/tree/main/Chapter12.

## Introducing Power Query

Each time you use Power Query, you will work through a series of at least three, and sometimes as many as five, phases:

- **Connecting**: Power Query enables you to connect to a variety of data sources, including worksheet ranges, workbooks, text files, PDF files, and databases.

> Nuance
> Any worksheet ranges that you connect to within an Excel workbook will automatically be converted into Tables.

- **Transforming**: The Power Query Editor enables you to shape your data, much like a potter shapes a lump of clay, but in this case, you can opt to remove unnecessary rows and columns, add columns, separate data from a single column into two or more columns, and much more.

- **Combining**: The optional combining phase enables you to stack different datasets together, such as combining individual worksheets from a workbook into a single list or merging data, which is akin to using lookup formulas in Excel to match related data from a second list into list, which I discuss in detail in Chapter 10, Lookup Functions and Dynamic Arrays.

- **Loading**: This phase returns the data to your Excel workbook and maintains a refreshable connection. Data is typically returned to Excel as a Table, so make sure that you read Chapter 7, Automating Common Excel Tasks with the Table Feature, to see how you can take your Power Query results further.

- **Refreshing**: Refreshing a Power Query connection means replacing the existing data in your workbook with a fresh copy from the data source. If you only have a one-time need to access a dataset, then you may not need to worry about refreshing. More often, though, the purpose of Power Query is to establish updatable, persistent connections to data sources. As you’ll see, you can refresh connections either manually or automatically.

Typically, you must only work through the steps in the first four phases once, as opposed to the unending work involved in manually cleaning up reports. You can, of course, edit the steps at any point. Another unique aspect of Power Query is that you will be able to walk back and forth through the steps of your transformations to see how the data looked both before and after you add another step to the process.

## Creating a list of worksheets

I can visualize situations where an auto-generated list of worksheets would be a helpful addition to many workbooks. A couple of ideas include tracking the progress of a workbook audit, eyeballing a simple list of all worksheets in a workbook, or maybe taking things up a level by building a clickable worksheet index. Such an index may feel redundant. After all, when right-click on the navigation arrows in the bottom left-hand corner of Excel you can navigate by double-clicking on any worksheet Name the Activate dialog box. On the other hand, this index will be a self-updating listing that includes the Names of hidden worksheets and can be sorted alphabetically if desired. Let's jump in:

1. Open the Chapter 12 - Workbook Index.xlsx example workbook for this chapter, which contains 17 worksheets.

2. Activate the Summary tab so that your index will appear as the first sheet in the workbook.

> Tip
> When you load data from Power Query into a workbook, a new worksheet will be created to the left of the active worksheet.

3. Choose Data | Get Data | From File | From Excel Workbook in Excel 2019 or later. Choose Data | New Query | From File | From Excel Workbook in Excel 2016. In Excel 2013 and 2010 you must download and install the free Power Query add-in before you can choose Power Query | From File | From Workbook in Excel 2013 and 2010.

4. Navigate to and select the Chapter 12 - Workbook Index.xlsx workbook again and then click Import. As you will see, a Power Query connection can refer to the workbook that you're working in. This may feel like you're creating a circular reference, but we're simply using the current workbook as our data source.

5. Choose Chapter 12 - Periodic Income Statement.xlsx from the list in the Navigator dialog box and click the Transform Data button shown in Figure 12.1. The Load button loads data that you've connected directly to Excel worksheets, while Transform Data launches the Power Query Editor so that you can make changes before returning the data to the workbook. The area that reads No data selected for preview gives you a glance at the data you've selected if you choose a Table or worksheet, but in this case, we've chosen the workbook itself.

> Nuance
> You cannot work in Excel when the Power Query Editor is open. As you'll see, you can return data to Excel at any time from Power Query and then circle back and continue editing in Power Query later.

## Tip

Depending on the type of data source you’re connecting to, you may see Tables, worksheets, or pages from PDF files in the list along the left. You can connect to any item in the list and, optionally, click Select multiple items to create two or more connections.

![](img/0c7a357df7e57e06b6d9f631c219040c_386_0.png)

Power Query Navigator window

## 6. The Power Query Editor will open, as shown in Figure 12.2:

![](img/0c7a357df7e57e06b6d9f631c219040c_386_1.png)

Figure 12.1 – The Power Query Editor

As shown in Figure 12.2. The Power Query Editor has the following aspects:

- Ribbon interface: Power Query uses a Ribbon concept similar to Microsoft Excel.
- Query listing: The task pane that appears along the left-hand side that lists all the connections that are present in the current workbook.
- Data preview: This is a grid that offers a preview of what your data will look like once you load it into an Excel worksheet.
- Query Settings task pane: The panel that appears along the right-hand side of the screen. It records each step that occurs in the data transformation.

7. Enter a new Name for the query, such as Index, in the Name field of the Query Settings task pane.

8. Right-click on the Name column and choose Delete Other Columns. This will leave a list of the worksheet Names in the Data Preview window, as shown here:

![](img/0c7a357df7e57e06b6d9f631c219040c_387_0.png)

Figure 12.2 – Power Query results before loading data into the worksheet

9. Choose Home | Close & Load | Load To to bring up the Import Data dialog box. As you can see, Power Query results can be viewed in several ways:

- Table: A range of cells formatted as a Table within a new worksheet in your workbook.

