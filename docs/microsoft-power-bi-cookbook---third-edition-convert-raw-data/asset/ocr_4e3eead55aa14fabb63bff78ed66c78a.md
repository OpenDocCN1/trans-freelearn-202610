+   - Backing up semantic models
+   - Leveraging Direct Lake
+   - Applying data science

By the end of this chapter, you will have a firm understanding of Microsoft Fabric and Power BI Premium capabilities and features.

# Technical Requirements

The following are required to complete the recipes in this chapter:

+   - Power BI Desktop.
+   - Microsoft Fabric or Power BI Premium capacity or trial.
+   - SQL Server 2022 or newer with the AdventureWorksDW2022 database installed. See the Installing SQL Server and the AdventureWorks Database recipe from Chapter 1, Installing and Licensing Power BI Tools.
+   - The files for this chapter can be downloaded from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition.

# Creating a Data Warehouse

Gartner defines a data warehouse as follows:

> “A data warehouse is a storage architecture designed to hold data extracted from transaction systems, operational data stores, and external sources. The warehouse then combines that data in an aggregate, summary form suitable for enterprise-wide data analysis and reporting for predefined business needs.”

Data warehouses have been a staple of business intelligence architectures for decades given their ability to deliver a central and single source of truth for use by a variety of applications and use cases. By handling data quality and integration needs in powerful upstream tools like Azure Data Factory pipelines and producing a consolidated set of dimension and fact tables with surrogate keys for relationships/joins via familiar SQL queries and support for historical tracking, a well-designed and managed data warehouse remains a highly valuable asset.

Regardless of the specific history of the term data warehouse, today the data warehouse is a staple of today’s enterprise business systems and a core component of business intelligence and reporting for many organizations. Microsoft Fabric provides data warehouse capabilities via cloud-based, lake-centric technologies built upon the SQL Server engine, distributed processing, and centralized storage within OneLake, Microsoft’s logical, centralized data lake for Fabric.

This recipe provides guidance for how to create and use a data warehouse within Microsoft Fabric.