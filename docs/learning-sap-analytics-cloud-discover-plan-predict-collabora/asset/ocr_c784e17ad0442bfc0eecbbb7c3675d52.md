# Planning and analytical models

In SAP Analytics Cloud, you can create planning and analytical models. A planning model is a full-featured model, which comes with preconfigured time and categories dimensions for actual, budget, planning, forecast, and rolling forecast. A planning model also supports multicurrency and offers security at the level of both model and dimension to restrict access to specific values in the report to named individuals. It also comes with auditing features for traceability. The users who have planning privileges can create their own versions of model data, use allocation features, add new data to the model, and copy and paste data into the model.

![](bbox=[0.08, 0.31, 0.15, 0.37])

*In contrast, analytical models are simpler and more flexible general-purpose BI models. Analytical models are not preconfigured with categories for budget and forecast data.*

In addition to these two models, you can connect to an SAP **high-performance analytic appliance (HANA)** database to import database views and create new models. You can create analytics models based on HANA views, which allow you to access dimensions from HANA. To create such models, the system administrator must first set up the HANA database. The analytic or calculation-type HANA views in the database can be imported to create new analytic-type models. A view can only be imported once; to re-import the view, delete the existing model from the Models list page, and then start the import process.

This chapter will teach you how to create a simple analytical model from an Excel file, while the planning model is the subject of Chapter 3, *Planning Model.*