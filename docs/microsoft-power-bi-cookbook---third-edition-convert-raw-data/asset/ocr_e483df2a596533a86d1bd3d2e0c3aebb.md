# Installing Additional Tools

Power BI professionals responsible for the development of semantic models (datasets) routinely utilize additional tools beyond Power BI Desktop to create and manage their models. For example, they use Tabular Editor to quickly implement changes to measures or add new objects, such as perspectives and calculation groups. Likewise, ALM Toolkit is routinely used to deploy incremental and metadata-only changes to Power BI.

Another such tool is DAX Studio, a third-party tool used to query data models, edit and format code, browse the structure and metadata of data models, and analyze the performance and execution characteristics of DAX queries. For larger and more complex data models and expressions, as well as projects involving multiple models, DAX Studio becomes an essential supplement to the development and performance-tuning processes.

Data Analysis Expressions (DAX) is the “language of Power BI,” as it is used to create the measures and queries visualized in Power BI reports and dashboards. Power BI generates and submits DAX queries to the source data model based on the structure of the visualization, user selections, and filters—just as other tools such as Excel generate MDX queries based on the selections and structure of pivot tables and slicers from workbooks. DAX expressions are also used to define security roles and can optionally be used to create columns and tables in data models, based on other tables and columns in the model, which can be refreshed at processing time and used by measures and queries. DAX serves the same function in Azure Analysis Services (AAS) and SQL Server Analysis Services (SSAS) tabular models, as well as Power Pivot for Excel models. It’s essential that BI professionals have a robust tool to develop and analyze DAX code and the data models containing these expressions.

# Getting ready

To prepare for this recipe, follow this step:

+   1. Download the current release from the DAX Studio website: https://daxstudio.org/downloads/. The installer version is a single file that provides a wizard-based installation and setup, while the portable version is a ZIP file that does not provide a wizard-based installation. The installer version is recommended.

You are now prepared to install DAX Studio.

# How to install DAX Studio

+   1. Use the Open file link or run the file from your Downloads folder. The file will be named something like DaxStudio_3_0_10_setup.exe.
+   2. Once the installation starts, select the option Install for all users (recommended).
+   3. If prompted by User Account Control, select the Yes button.
+   4. Accept the license agreement and click the Next button.
+   5. Choose a folder path to install the tool and click the Next button.
+   6. Choose whether the DAX Studio add-in for Excel will also be installed. Click the Next button. Note that: