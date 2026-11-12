
## Aggregation

The following exercise will use two aggregates from a single data source, one at the **State** level and the other at the **City** level. These aggregated data sources will be used to create two worksheets. Each of these worksheets will be placed on a dashboard along with a third worksheet with row-level information. Finally, filter actions will be created to tie the three worksheets together. The purpose of the exercise is to demonstrate how small extracts might be used in conjunction with a larger dataset to create a more performant dashboard:

- 1. Open the workbook associated with this chapter and navigate to the State Agg worksheet
- 2. In the **Data** pane, select the State Agg data source
- 3. Create a filled map using state by placing **State** on the **Detail** shelf and selecting **Filled Map** from the **Marks** card
- 4. Right-click on the State Agg data source and select **Extract Data**
- 5. Note that Tableau displays an error stating that it cannot find the referenced file. You can either point to the instance of **Superstore** that ships with Tableau or you can use the instance provided via the GitHub link: https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter12.
- 6. After connecting to the data source, Tableau will display the **Extract Data** dialog box. Within the dialog box, select **Aggregate data for visible dimensions** and **All rows**. Click the **Hide All Unused Fields** button and then click on **Extract**. Note that the resulting extract only contains **State**. Also note that the data has been aggregated so that no underlying data is available.
- 7. Navigate to the City Agg worksheet
- 8. In the **Data** pane, select the City Agg data source. Note that this data source has already been extracted and so only contains **State**, **City**, and **Sales**. Also note that the data has been aggregated so that no underlying data is available.
- 9. Place **City** on the **Rows** shelf, **Sales** on the **Text** shelf, and **State** on the **Detail** shelf. Don't forget to include **State** even though it does not display on the view. It must be used so that the dashboard created at the end of the exercise works correctly.
- 10. Navigate to the Row Detail worksheet and select the Superstore dataset
- 11. Create a crosstab view that displays **Customer Name**, **Order ID**, **Row ID**, **Profit**, and **Sales**. One quick way to create this view is to double-click on each field.
- 12. Navigate to the Agg Dash dashboard and place each of the three worksheets on the dashboard
- 13. Create a blend relationship between **City** and **State** from the **Superstore** and **City Agg** data source:

![](img/66cf6b89349fda3581a4f196cb3c98a4_561_0.png)

Figure 12.12: Blend

- 14. Create the following action via Dashboard | Actions | Add Action | Filter:

![](img/66cf6b89349fda3581a4f196cb3c98a4_561_1.png)

Figure 12.13: Adding City to Detail action

- 15. Create the following action via Dashboard | Actions | Add Action | Filter:

![](img/66cf6b89349fda3581a4f196cb3c98a4_562_0.png)

Figure 12.14: Adding State to City action

- 16. After creating these two actions, in the dashboard, click on the State field
- 17. Then, click on the City field
- 18. Click on a blank portion of the City Agg worksheet to exclude all values on Row Detail
- 19. Click on a blank portion of the State Agg worksheet to exclude all values on City Agg
- 20. Format the dashboard as desired:

![](img/66cf6b89349fda3581a4f196cb3c98a4_562_1.png)

Figure 12.15: Action on dashboard

Having completed this exercise, note that the resulting dashboard is performant for the following reasons:

- When the user first opens the dashboard, only State Agg displays. This is performant for two reasons. First, displaying a single worksheet as opposed to every worksheet when opening the dashboard causes fewer initial queries and less rendering. Second, accessing a small extract is quicker than accessing a larger data source.

- Since the City Agg worksheet is also accessing a small extract, when the user clicks on a state, the **City Agg** worksheet will appear quickly.

- When the user clicks on **City**, a call is made to the data source that only includes the information for that city. A relatively small amount of data is pulled, and performance should be good for even larger datasets.

Another aspect of good performance practice, apart from using aggregate extracts, should be considered for this exercise. The dashboard contains no quick filters. Often, using quick filters on a dashboard is unnecessary. If the worksheets on the dashboard can be used to filter, those worksheets can essentially do double duty. That is to say, worksheets can provide valuable analysis while simultaneously acting as filters for other worksheets on the dashboard. This represents a performance improvement over using quick filters since adding quick filters would cause additional queries to be sent to the underlying data source.

In the preceding dashboard, each worksheet references a different data source. Therefore, you may ask, how are the action filters able to function across the different data sources? The answer can be found in the filter action dialog box. As shown in **Figure 12.13** and **Figure 12.14**, **All** fields are considered **Target Filters**. Tableau simply matches any fields of the same name across each data source. Extracts can be optimized for even better performance results. We'll cover this in the next section.

## Optimizing extracts

Optimization accelerates performance by materializing calculated fields when possible. This means that Tableau generates values for calculated fields in the extract so that those values can be looked up instead of calculated. If you were to use table calculations, Tableau would have to calculate the values each time you change the view.

**Note**

Note that not all calculated fields materialize. Fields that are not materialized include table calculations, changeable or unstable functions, such as `NOW()` and `TODAY()`, and calculated fields using parameters.

When an extract is first created, it is automatically optimized. In other words, calculated fields automatically materialize when possible. However, over the course of time, calculated fields may be altered, which will cause the extract to drop materialized fields. At such times, open **Data | [Data Source] | Extract** and click on **Compute Calculations Now** in order to regenerate the materialized fields:

![](img/66cf6b89349fda3581a4f196cb3c98a4_564_0.png)

Figure 12.16: Computing calculations

If an extract is set to refresh on Tableau Server, the extract is automatically optimized for each refresh.

Finally, if you make use of parameters in your dashboard, check whether you can eliminate those and use calculations instead to improve performance. Also, split calculations if they can't be materialized as a whole. Put the part that can be materialized in one calculated field and the non-materialized part in another. If parts of the calculation can be calculated within the extract creation, you will gain performance.

The advantage of using extracts has now been discussed in great detail, so let's move on and see how we can make the most out of filters.

## Using filters wisely

Filters generally improve performance in Tableau. For example, when using a dimension filter to view only the West region, a query is passed to the underlying data source, resulting in information being returned for just that region. By reducing the amount of data returned, performance improves. This is because less data means reduced network bandwidth load, reduced database processing requirements, and reduced processing requirements for the local computer.

Filters can also negatively impact Tableau’s performance. For example, using **only relevant values** causes additional queries to be sent to the underlying data source, thereby slowing down the response time. Also, creating quick filters from high-cardinality dimensions can impair performance.

Tableau’s filters are executed in a specific order, so keep this in mind when using them. The following flowchart, along with a link to an hour-long presentation, may help you grasp the concept fully: https://help.tableau.com/current/pro/desktop/en-us/order_of_operations.htm:

![](img/66cf6b89349fda3581a4f196cb3c98a4_565_0.png)

Figure 12.17: Filtering the order of operations

The rest of this section follows, step by step, the order of operations. By the end of it, you will know which filters to use in which situations so as to achieve the best performance for your dashboards.

### Extract filters

Extract filters remove data from the extracted data source. Simply put, the data isn’t there. Thus, performance is enhanced by reducing the overall amount of data. Performance may also be improved since extracted data uses Tableau’s proprietary columnar dataset.

Furthermore, extracts are always flattened, which will have performance advantages over connecting to datasets using joins. To create an extract filter, begin by selecting **Data | [Data Source] | Extract Data**. In the resulting dialog box, choose to add a filter.
