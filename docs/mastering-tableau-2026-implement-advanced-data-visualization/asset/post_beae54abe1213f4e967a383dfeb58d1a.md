
- 3. Add a relationship between the **Shirts** and **Pants** data sources for **Material Type**, taking the following steps:

- 1. Select **Data** and then **Edit Relationships**.
- 2. Ensure that **Shirts** is the primary data source and **Pants** is the secondary data source.
- 3. Select the **Custom** radio button.
- 4. Click **Add...**
- 5. Select **Material Type** in both the left and right columns:

![](img/66cf6b89349fda3581a4f196cb3c98a4_163_0.png)

Figure 4.28: Custom blend relationships

- 6. Click **OK** to return to the view.
- 4. Place **Material Type** on the **Rows** shelf.
- 5. Select the **Pants** data source and make sure that the chain-link icon next to **Material Type** in the **Data** pane is activated and that the chain-link icon next to **Salesperson** is deactivated. If the icon is a gray, broken chain link, it is not activated. If it is an orange, connected chain link, it is activated:

![](img/66cf6b89349fda3581a4f196cb3c98a4_164_0.png)

Figure 4.29: Shirts and Pants

- 6. Place Material Cat before Material Type, and Fastener after Material Type on the Rows shelf as follows:

![](img/66cf6b89349fda3581a4f196cb3c98a4_164_1.png)

### Adding Secondary Dimension

| Material Cat | Material Type | Fastener | |
|---|---|---|---|
| Tech | Acrylic | Button | Abc |
| | Polyester | Velcro | Abc |
| Traditional | Wool | * | Abc |

Figure 4.30: Secondary dimensions

Material Cat is a dimension from a secondary data source. Data blending does not enable dimensions from a secondary data source. Why does it work in this case? There are a few reasons:

- There is a one-to-many relationship between **Material Cat** and **Material Type**; that is, each member of the **Material Type** dimension is matched with one and only one member of the **Material Cat** dimension.
- The view is blended on **Material Type**, not **Material Cat**. This is important because **Material Type** is at a lower level of granularity than **Material Cat**. Attempting to blend the view on **Material Cat** will not enable **Material Type** as a secondary dimension.
- Every member of the **Material Type** dimension within the primary data source also exists in the secondary data source.

**Fastener** is also a dimension from the secondary data source. Figure 4.35 displays * in one of the cells, thus demonstrating that **Fastener** is not working as a dimension should; that is, it is not slicing the data, as discussed in Chapter 1, Reviewing the Basics. The reason an asterisk is displayed is that there are multiple **Fastener** types associated with **Wool**. **Button** and **Velcro** display because **Acrylic** and **Polyester** each have only one **Fastener** type in the underlying data.

If you use blending, make sure that your main reason is to combine measures and that you do not need the dimensions on a detailed level. It is especially useful to know this before you create a dashboard, to prepare accordingly. Your data needs extra prepping (check Chapter 3, Using Tableau Prep Builder) because neither a join nor a blend can bring you the expected data structure. Alternatively, you can make use of scaffolding, a technique that uses a helper data source—we will discuss this in the next section.

## Introducing scaffolding

**Scaffolding** is a technique that introduces a second data source through blending to reshape and/or extend the initial data source. Scaffolding enables capabilities that extend Tableau to meet visualization and analytical needs that may otherwise be exceedingly difficult or altogether impossible. Joe Mako, who pioneered scaffolding in Tableau, tells a story in which he used the technique to recreate a dashboard using four worksheets. The original dashboard, which did not use scaffolding, required 80 worksheets painstakingly aligned pixel by pixel.

Among the many possibilities that scaffolding enables is extending Tableau's forecasting functionality. Tableau's native forecasting capabilities are sometimes criticized for lacking sophistication. Scaffolding can be used to address this criticism.

The following are the steps:

- 1. In the workbook associated with this chapter, select the **Scaffolding** worksheet and connect to the **World Indicators** data source.
- 2. Using Excel or a text editor, create a **Records** dataset. The following two-row table represents the **Records** dataset in its entirety:

| | A | B |
|---|---|---|
| 1 | Record | |
| 2 | | 1 |
| 3 | | 2 |
| 4 | | |
| 5 | | |

Figure 4.31: Excel file

- 3. Connect Tableau to the dataset.
- 4. To be expedient, consider copying the dataset directly from Excel by using Ctrl + C and pasting it directly into Tableau with Ctrl + V.
- 5. Create a Start Date parameter in Tableau, with the settings seen in the following screenshot. Notice the highlighted sections in the screenshot, by which you can set the desired display format:

![](img/66cf6b89349fda3581a4f196cb3c98a4_166_0.png)

Figure 4.32: Display format

- 6. Create another parameter named End Date with identical settings.
- 7. In the Data pane, right-click on the Start Date and End Date parameters you just created and select Show Parameter:

![](img/66cf6b89349fda3581a4f196cb3c98a4_167_0.png)

Figure 4.33: Show Parameter

- 8. Set the start and end dates as desired; for example, 2000-2027:

![](img/66cf6b89349fda3581a4f196cb3c98a4_167_1.png)

Figure 4.34: Start and end dates

- 9. Select the Records data source and create a calculated field called Date with the following code:

```
IIF([Records]=1,[Start Date],[End Date])
```

- 10. Place the Date field on the Rows shelf.
- 11. Right-click on the Date field on the Rows shelf and select Show Missing Values. Note that all the dates between the start and end date settings now display:

![](img/66cf6b89349fda3581a4f196cb3c98a4_168_0.png)

Figure 4.35: Show Missing Values

- 12. Create a parameter named Select Country A with the settings shown in the following screenshot. Note that the list of countries was added with the Add values from button:

Edit Parameter [Select Country A]

Name Select Country A Properties Data type String Current value China Allowable values List

| Value | Display As |
| --- | --- |
| Afghanistan | Afghanistan |
| Albania | Albania |
| Algeria | Algeria |
| American Samoa | American Samoa |
| Andorra | Andorra |
| Angola | Angola |
| Antigua and Barbuda | Antigua and Barbuda |

- Events
- Fifa2022
- Join
- Pants
- Patient_Snapshot
- Sample - EU Superstore Join
- Sample - EU Superstore Relationship
- Shirts
- World Indicators
- Parameters
- Paste from clipboard

Figure 4.36: Adding values from a field

- 13. Create another parameter named Select Country B with identical settings.

- 14. Create a parameter named **Select Country A Forecast** with the settings given in the following screenshot:

![](img/66cf6b89349fda3581a4f196cb3c98a4_170_0.png)

Figure 4.37: Editing a parameter

- 15. Create another parameter named **Select Country B Forecast** with identical settings.

- 16. In the Data pane, right-click on the four parameters you just created (Select Country A, Select Country B, Select Country A Forecast, and Select Country B Forecast) and select Show Parameter:

![](img/66cf6b89349fda3581a4f196cb3c98a4_171_0.png)

Figure 4.38: Show Parameter

- 17. Make sure that the Date field in the World Indicators data source has the orange chain-link icon deployed. This indicates it is used as a linking field:

- Rentals_Snapshot
- Records
- Sample - EU Superstore Join
- Sample - EU Superstore Relationship
- Shirts
- World Indicators

Search

Folders

- Population 0-14
- Population 15-64
- Population 65+
- Population Total
- Population Urban

- Country
- Date
- Region
- Measure Names

Figure 4.39: Link

- 18. Within the World Indicators dataset, create the following calculated fields:

Country A Population:

```
IIF([Country] = [Select Country A],[Population Total] ,NULL)
```

Country B Population:

```
IIF([Country] = [Select Country B],[Population Total] ,NULL)
```

- 19. Within the Records dataset, create the following calculated fields:

Actual/Forecast:

```
IIF(ISNULL(AVG([World Indicators].[Population Total])),"Forecast","Actual")
```
