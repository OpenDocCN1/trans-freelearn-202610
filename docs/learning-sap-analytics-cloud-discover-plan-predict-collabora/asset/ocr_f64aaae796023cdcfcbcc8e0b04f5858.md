# Data sources and models

Before commencing your analytical tasks in SAP Analytics Cloud, you need to create models. A model is the foundation of every analysis you create to evaluate the performance of your organization. It is a high-level design that exposes the analytic requirements of end users. Planning and analytics are the two types of models you can create in SAP Analytics Cloud.

Analytics models are simpler and more flexible, while planning models are full-featured models in which you work with planning features. Preconfigured with dimensions for time and categories, planning models support multicurrency and security features at both model and dimension levels. Chapters 2 and 3 of this book are dedicated to these two models.

To determine what content to include in your model, you must first identify the columns from the source data on which users need to query. The columns you need in your model reside in some sort of data source. SAP Analytics Cloud supports three types of data sources: files (such as CSV or Excel files) that usually reside on your computer, live data connections from a connected remote system, and cloud apps.

In addition to the files on your computer, you can use on-premise data sources, such as SAP Business Warehouse, SAP ERP, SAP Universe, SQL database, and more, to acquire data for your models. In the cloud, you can get data from apps such as Concur, Google Drive, SAP Business ByDesign, SAP Hybris Cloud, OData Services, and Success Factors. The following figure depicts these data sources. The cloud app data sources you can use with SAP Analytics Cloud are displayed above the firewall mark, while those in your local network are shown under the firewall.

As you can see in the following figure, there are over twenty data sources currently supported by SAP Analytics Cloud. The methods of connecting to these data sources also vary from each other. Due to space constraint, it is not possible to outline every connecting method in this chapter. However, some instances provided in this chapter would give you an idea on how connections are established to acquire data. The connection methods provided here relate to on-premise and cloud app data sources. Considering the audience of this book who may not have access to these data sources, and to keep things simple, Chapter 2, Models in SAP Analytics Cloud, will guide them to create an analytics model in the trial version of SAP Analytics Cloud using an Excel file provided with this book.

![](bbox=[0.08, 0.78, 0.16, 0.84])

> *Due to relevance with the topic, the sub-sections that follow are added here to provide an overview of different types of connections you can establish in SAP Analytics Cloud to build your models.*