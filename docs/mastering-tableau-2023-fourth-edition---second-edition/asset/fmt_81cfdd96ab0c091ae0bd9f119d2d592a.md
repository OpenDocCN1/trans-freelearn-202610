
# Forecasting Population Growth

To demonstrate this, let us ask, "When will India's population overtake China's?" You can set the parameters as desired—I chose 0.45% and 1.20% as the average yearly growth rates, but feel free to choose any parameter you think works best for a country. In *Figure 4.48*, you see that with a growth rate of 0.45% and 1.20% for China and India, respectively, India will have more inhabitants than China by 2024. You can observe this by looking at the columns `Country A Population` and `Country B Population`. Everything in orange is a forecast, while everything in blue is actual data from our dataset:

![](Images/B18435_04_43.png)

**Figure 4.43: Forecast dashboard**

We are obviously already many years ahead; can you use this dashboard to figure out the actual average growth rate for China and India from 2012 to 2020 if I tell you that the population in 2020 was 1,439,323,776 in China and 1,380,004,385 in India? Share your results on Tableau Public with the tag `#MasteringTableau`!

## Data Scaffolding

One key to this exercise is data scaffolding. Data scaffolding produces data that does not exist in the data source. The `World Indicators` dataset only includes dates from 2000 to 2012 and obviously, the `Records` dataset does not contain any dates. By using the `Start Date` and `End Date` parameters coupled with the calculated `Date` field, we were able to produce any set of dates desired. We had to blend the data, rather than join or union, to keep the original data source intact and create all additional data outside of `World Indicators` itself.

The actual data scaffolding occurred upon selecting `Show Missing Values` from the `Date` field dropdown after it was placed on the `Rows` shelf. This allowed every year between `Start Date` and `End Date` to display, even when there were no matching years in the underlying data. *Chapter 6*, *Utilizing OData, Data Densification, Big Data, and Google BigQuery*, will explore something remarkably similar to data scaffolding, data densification, in more detail.

## Calculated Fields Deep Dive

Let us look at a few of the calculated fields in more depth to better understand how forecasting works in this exercise:

### Calculated field: `Actual/Forecast`

```
IIF(ISNULL(AVG([World Indicators].[Population Total])),"Forecast","Actual")
```

The preceding code determines whether data exists in the `World Indicators` dataset. If the date is after 2012, no data exists and thus `Forecast` is returned.

### Calculated field: `Country A Population`

```
IF [Actual/Forecast] = "Actual" THEN SUM([World Indicators].[Country A Population]) ELSE PREVIOUS_VALUE(0)
*[Select Country A Forecast] + PREVIOUS_VALUE(0) END
```

If forecasting is necessary to determine the value (that is, if the date is after 2012), the `ELSE` portion of this code is exercised. The `PREVIOUS_VALUE` function returns the value of the previous row and multiplies the results by the forecast and then adds the previous row.

Let's look at an example in the following table:

| `Previous Row Value (PRV)` | | 1,000 |
| --- | --- | --- |
| `Forecast (F)` | | 0.01 |
| `PRV * F` | | 10 |
| `Current Row Value` | | 1,010 |

One important thing to note in the `Country A Population` calculated field is that the forecast is quite simple: multiply the previous population by a given forecast number and tally the results. Without changing the overall structure of the logic, this section of code could be modified with more sophisticated forecasting.

These exercises have shown that with a few other tricks and techniques, blending can be used to great effect in your data projects. Finally, we will talk about data structures in general such that you will better understand why Tableau is doing what it is doing and how you can achieve your visualization goals.

# Understanding Data Structures

The right data structure is not easily definable. True, there are ground rules. For instance, tall data is generally better than wide data. A wide dataset with lots of columns can be difficult to work with, whereas the same data structured in a tall format with fewer columns but more rows is usually easier to work with.

But this is not always the case! Some business questions are more easily answered with wide data structures. And that is the crux of the matter. Business questions determine the right data structure. If one structure answers all questions, great! However, your questions may require multiple data structures. The `pivot` feature in Tableau helps you adjust data structures on the fly to answer different business questions.

Before beginning this exercise, make sure you understand the following points:

- Pivoting in Tableau is limited to Excel, text files, and Google Sheets; otherwise, you must use Custom SQL or Tableau Prep
- A pivot in Tableau is referred to as *unpivot* in database terminology

As a business analyst for a hospital, you are connecting Tableau to a daily snapshot of patient data. You have two questions:

- How many events occur on any given date? For example, how many patients check in on a given day?
- How much time expires between events? For example, what is the average stay for patients in the hospital for multiple days?

To answer these questions, take the following steps:

1. In the starter workbook associated with this chapter, select the `Time Frames` worksheet, and within the **Data** pane, select the `Patient_Snapshot` data source.
2. Click on the dropdown in the **Marks** card and select **Bar** as the chart type.
3. Right-click in the **Data** pane to create a parameter named `Select Time Frame` with the settings displayed in the following screenshot:

![](Images/B18435_04_44.png)

**Figure 4.44: Parameter**

4. Right-click on the parameter we just created and select `Show Parameter`.
5. Right-click in the **Data** pane to create a calculated field called `Selected Time Frame` with the following code:

```
CASE [Select Time Frame]
WHEN 1 THEN DATEDIFF('day',[Check-in Date],[Discharge Date])
WHEN 2 THEN DATEDIFF('day',[Surgery Date],[Discharge Date])
WHEN 3 THEN DATEDIFF('day',[Check-in Date],[Surgery Date])
END
```

6. Drag the following fields to the associated shelves and define them as directed:

| Field name | Shelf directions |
| --- | --- |
| `Patient Type` | Drag to the **Filter** shelf and check **Inpatient**. |
| `Check-in Date` | Drag to the **Filter** shelf and select **Range of dates**. Also, right-click on the resulting filter and select **Show Filter**. |
| `Check-in Date` | Right-click and drag to the **Columns** shelf and select **MDY**. |
| `Selected Time Frame` | Right-click and drag to the **Rows** shelf and select **AVG**. |
| `Selected Time Frame` | Right-click and drag to the **Color** shelf and select **AVG**. Set colors as desired. |

7. After these actions, your worksheet should look like the following:

![](Images/B18435_04_45.png)

**Figure 4.45: Initial dashboard**

8. Right-click on the `Avg Selected Time Frame` axis and select `Edit Axis…`, as shown in the following figure. Then, delete the title:

![](Images/B18435_04_46.png)

**Figure 4.46: Edit Axis...**

9. Select **Worksheet** | **Show Title**. Edit the title by inserting the parameter `Select Time Frame`, as shown in the following screenshot:

![](Images/B18435_04_47.png)

**Figure 4.47: Add parameter**

The data structure was ideal for the first part of this exercise. You were able to create the visualization quickly. The only section of moderate interest was setting up the `Selected Time Frame` calculated field with the associated parameter.

This allows the end user to choose which time frame they would like to view:

![](Images/B18435_04_48.png)

**Figure 4.48: Dashboard**

But what happens if you need to find out how many people were involved in a type of event per day?
