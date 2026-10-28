# RLS in composite models with DirectQuery to Power BI datasets

Now that we have a better understanding of how composite models work with a DirectQuery connection to Power BI datasets, SSAS tabular, or AAS models, it is time to take this another step further and discuss more technical scenarios that include RLS. We’ll use the AdventureWorksDW2019 database for this scenario.

The business has an Internet Sales dataset published to the Power BI service. The following image shows the Internet Sales data model:

![](bbox=[0.17, 0.38, 0.83, 0.62])

Figure 14.13: Internet Sales data model

Note the one-to-many relationship between the DimSalesTerritory and DimEmployee tables. The developer created a dynamic RLS so each salesperson can see only their relevant territory’s sales values. To ensure the DimEmployee table always shows the data that is relevant to the report users, we have to use the following DAX expression to create a rule on the DimEmployee table:

```
[EmailAddress] = USERPRINCIPALNAME ()
```