# Selecting a source file

You can import data from an external file, such as an Excel spreadsheet or a comma-separated-values file, into a new model. If the data is saved in separate sheets in the Excel workbook, you will see a drop-down list from where you can choose which sheet to import:

![](bbox=[0.23, 0.23, 0.77, 0.45])

The source data must include columns that can be used to create dimensions in a new model, and it must also include transactional data (measures). The transactional data is not visible in modeler, only in the output that is generated.

You can connect to other systems to create a new model from an external source. To connect to such external sources, you have to select the use a datasource or get data from an app option. You can also create a model manually by setting up the dimensions individually and defining all the preferences and details of the model exactly as you require. You can create a model using the templates available in the planning wizard available within when you add a table to a story.

Note that some of these model creation options are not available in the trial version. So, in order to keep things simple, you are creating the model using an Excel file. The Bocage.xlsx file is a simple Excel file being provided with this book for demonstration purpose. It contains measures data (Quantity sold, Sales Revenue, Gross Margin, Discount, and Original Sales Price) to gauge the performance using date, state, city, store, manager, product, and category dimensions. In addition to measures and dimensions, the file has two columns (latitude and longitude) that will help you to visualize data in geo maps. This file can be downloaded from https://github.com/PacktPublishing/Learning-SAP-Analytics-Cloud.