
## Creating a new analytical model

Execute the following steps to create a new analytical model. The screenshot has numbered steps for sequence to be followed for creating an analytical model:

![](img/c69568baeee565fe02397867c05883e1_61_0.png)

## Selecting a source file

You can import data from an external file, such as an Excel spreadsheet or a comma-separated-values file, into a new model. If the data is saved in separate sheets in the Excel workbook, you will see a drop-down list from where you can choose which sheet to import:

![](img/c69568baeee565fe02397867c05883e1_62_0.png)

The source data must include columns that can be used to create dimensions in a new model, and it must also include transactional data (measures). The transactional data is not visible in modeler, only in the output that is generated.

You can connect to other systems to create a new model from an external source. To connect to such external sources, you have to select the use a datasource or get data from an app option. You can also create a model manually by setting up the dimensions individually and defining all the preferences and details of the model exactly as you require. You can create a model using the templates available in the planning wizard available within when you add a table to a story.

Note that some of these model creation options are not available in the trial version. So, in order to keep things simple, you are creating the model using an Excel file. The Bocage.xlsx file is a simple Excel file being provided with this book for demonstration purpose. It contains measures data (Quantity sold, Sales Revenue, Gross Margin, Discount, and Original Sales Price) to gauge the performance using date, state, city, store, manager, product, and category dimensions. In addition to measures and dimensions, the file has two columns (latitude and longitude) that will help you to visualize data in geo maps. This file can be downloaded from https://github.com/PacktPublishing/Learning-SAP-Analytics-Cloud.

## Setting name, description, and other parameters for the model

After a while, you will see the Data Integration screen carrying the uploaded data in the DataView tab, as shown in the following screenshot:

![](img/c69568baeee565fe02397867c05883e1_63_0.png)

The initial column header names from the source data are shown together with symbols (A) to show the data type that has been recognized. The data columns in the source file are used to create dimensions and measures in the model. The import process analyzes the source data and creates an initial data view with proposed dimensions for the new model.

After this auto-mapping process, you can refine the proposal by specifying dimension types and fixing any data-quality problems, which you will do next. In the Details panel, review the model name (B), and change it if desired. You can also type the optional description (C). Specify whether you want to use the first row of the data as column headers (D), which you should in the current scenario. You can edit the column names by double-clicking on the column headers or by changing the column names in the Details panel. Specify whether you want the model to be Planning Enabled (E).

Specify whether you want empty ID cells (F) to be filled with automatically generated values. If you clear this check box, rows with empty ID cells will be omitted.

## Setting dimension type

Once the data has been imported, you need to change some dimension types to ensure they deliver the result you intend to get from the model:

![](img/c69568baeee565fe02397867c05883e1_64_0.png)

Further refining of attributes can be achieved as described in the following screenshot:

After switching the Type attribute, the header icons of the two columns will also change.

![](img/c69568baeee565fe02397867c05883e1_65_0.png)

11

Change the Type attribute of Latitude and Longitude columns from Measure to Dimension. You can add multiple dimensions of this type to a model. Note that the import process has already set the type of State, City, Store, Manager, Product, and Category columns to dimension.

![](img/c69568baeee565fe02397867c05883e1_65_1.png)

12

Change the Type attribute of Quantity sold column from Dimension to Measure. In the initial data import, columns containing text are typically identified as Dimensions, and numeric data may be identified as Measures. Ensure that the Type attribute for Quantity Sold, Sales Revenue, Gross Margin, Discount, and Original Sales Price columns is marked as Measure.

## Creating a location dimension

You can enrich the model by creating a location dimension using latitude and longitude columns in your dataset, which will enable users to visualize the data in geo maps. Here are the steps to create a location dimension:

![](img/c69568baeee565fe02397867c05883e1_66_0.png)

## Deleting unnecessary columns from the model

You should keep your model clean from unwanted data. In this step, you will remove a couple of redundant columns from the model:

![](img/c69568baeee565fe02397867c05883e1_67_0.png)

## Creating a hierarchical relationship between two columns

**Hierarchies** are useful when there is a hierarchical relationship between two columns, such as category and product. Setting the columns up as a hierarchy allows us to drill up and down between the category and product values in charts. If hierarchical dimensions are included in a chart, you can drill up or down through dimensions to explore the data at different levels.

You use a dimension attribute called Parent-Child Hierarchy (Parent) to define a hierarchy for any generic dimension. For example, if your imported data contains two columns, Country and City, you can set the Country column to be the parent of the Country-City hierarchy.

In the current scenario, you will create a hierarchical relationship between the Category and Product columns available in the imported data:

![](img/c69568baeee565fe02397867c05883e1_69_0.png)

Click the Product column to see its attributes in the Details panel.

Next, click the Add Dimension Attributes link.

Select Parent-Child Hierarchy (Parent) option from the list. This will add a drop-down list labeled PARENT-CHILD HIERARCHY (PARENT).

Select the Category column from the drop-down list to set this column as parent of the Product column.

The following step ensures that the Category column is marked as parent of the Product column:

Click the Category column and see that this column is marked as parent of the Product column.

## Modeling

+   - Carbonated
+   - Juices
+   - Others
+   - Carbonated
+   - Juices
+   - Others
+   - Carbonated
+   - Juices
+   - Others
+   - Carbonated
+   - Juices
+   - Others

COLUMN
Category
TYPE
Parent-Child Hierarchy (Parent)
+ Make this a Dimension
PARENT OF DIMENSION
Product

After creating the model you will see this hierarchical relationship in the Modeler interface.

NAME
Bocage
DESCRIPTION
optional description here
MAPPING QUALITY
Mapping complete
DATA QUALITY
No data quality issues detected.
Use First Row as Column Headers
Planning Enabled
Fill applicable empty ID cells with a default value?
DEFAULT CURRENCY FOR MODEL
USD
Create Model

Finally, click the Create Model button in the Details panel followed by another Create button that appears in a separate dialog box to complete the model creation process.

## Viewing the model in the modeler interface

Once your model is created, you are taken to the modeler area with your new model appearing in the interface. It has a tab carrying two built-in dimensions (time and account) and five private dimensions (state, city, store, manager, and product), which are created using the source data file.

## The time dimension

Click on the Time dimension tab (A). The Time dimension is built into analytical models that define the start and end dates of the plan's timeline and the smallest time units that will be used in the plan. The time dimension specifies the overall time frame for the model and also the granularity—the smallest time period down to which the data will be analyzed--for example, year, quarter, month, or day.

*Note that in the current scenario, the lowest granularity and the two date range filters are applied automatically based on the time granularity defined in the underlying model (see the Date column format in the `Bocage.xlsx` file).*

These filters will ensure that only the details in the selected time period are visible, as is evident from the following screenshot:

![](img/c69568baeee565fe02397867c05883e1_72_0.png)
