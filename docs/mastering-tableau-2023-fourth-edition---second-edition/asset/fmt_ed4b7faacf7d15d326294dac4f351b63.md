
# Hyper takeaways

To sum up, `Hyper` is a highly specialized database engine that allows us as users to get the best out of our queries. If you recall, in `Chapter 1`, `Reviewing the Basics`, we already saw that every change on a sheet or dashboard, including drag and drop pills, filters, and calculated fields, among others, is translated into a query. Those queries are pretty much `SQL` lookalikes; however, in Tableau we call the querying engine `VizQL`.

`VizQL`, another hidden gem on your Tableau Desktop, is responsible for visualizing data in a chart format and is fully executed in memory. The advantage is that no additional space on the database side is required here. `VizQL` is generated when a user places a field on a shelf. `VizQL` is then translated into `SQL`, `MDX`, or `Tableau Query Language` (`TQL`) and passed to the backend data source with a driver.

## Focusing on data preparation

Tableau can be used effectively with various data preparation phases. Unfortunately, a single chapter is not sufficient to thoroughly explore how Tableau can be used in each phase. Indeed, such a thorough exploration may be worthy of an entire book! Our focus, therefore, will be directed toward data preparation, since that phase has historically accounted for up to 60% of the data mining effort. Our goal will be to learn how Tableau can be used to streamline that effort.

### Surveying data

Tableau can be a very effective tool for simply surveying data. Sometimes in the survey process, you may discover ways to clean the data or populate incomplete data based on existing fields. Sometimes, regretfully, there are simply not enough pieces of the puzzle to put together an entire dataset. In such cases, Tableau can be useful to communicate exactly what the gaps are, and this, in turn, may incentivize the organization to more fully populate the underlying data.

In this exercise, we will explore how to use Tableau to quickly discover the percentage of null values for each field in a dataset. Next, we’ll explore how data might be extrapolated from existing fields to fill in the gaps.

#### Establishing null values

The following are the steps to survey the data:

1. If you haven’t done so just yet, navigate to `https://public.tableau.com/profile/marleen.meier` to locate and download the workbook associated with this chapter.
2. Navigate to the worksheet entitled `Surveying` `&` `Exploring` `Data` and select `Happiness Report` data source.
3. Drag `Region` and `Country` to the `Rows` shelf. Observe that in some cases the `Region` field has `Null` values for some countries:
   ![](Images/B18435_02_01.png)
   Figure 2.1: Null regions
4. Right-click and `Edit` the parameter entitled `Select Field`. Note that the `Data Type` is set to `Integer` and we can observe a list that contains an entry for each field name in the dataset:
   ![](Images/B18435_02_02.png)
   Figure 2.2: Editing a parameter
5. In the `Data` pane, right-click on the parameter we just created and select `Show Parameter` Control.
6. Create a calculated field entitled `% Populated` and write the following calculation:
   ```
   SUM([Number of Records]) / TOTAL(SUM([Number of Records]))
   ```
7. In the `Data` pane, right-click on `% Populated` and select `Default Properties` | `Number Format…`:
   ![](Images/B18435_02_03.png)
   Figure 2.3: Adjusting default properties
8. In the resulting dialog box, choose `Percentage`.
9. Create a calculated field entitled `Null & Populated` and add the following code. Note that the complete case statement is fairly lengthy but also repetitive.
   In cases requiring a lengthy but repetitive calculation, consider using Excel to more quickly and accurately write the code. By using Excel’s `CONCATENATE` function, you may be able to save time and avoid typos.
   In the following code block, the code lines represent only a percentage of the total but should be sufficient to enable you to produce the whole block:
   ```
   CASE [Select Field]
   WHEN 1 THEN IF ISNULL ([Country]) THEN 'Null Values' ELSE 'Populated Values' END
   WHEN 2 THEN IF ISNULL ([Region]) THEN 'Null Values' ELSE 'Populated Values' END
   WHEN 3 THEN IF ISNULL ([Economy (GDP per Capita)]) THEN 'Null Values' ELSE 'Populated Values' END
   WHEN 4 THEN IF ISNULL ([Family]) THEN 'Null Values' ELSE 'Populated Values' END
   WHEN 5 THEN IF ISNULL ([Freedom]) THEN 'Null Values' ELSE 'Populated Values' END
   WHEN 6 THEN IF ISNULL ([Happiness Rank]) THEN 'Null Values' ELSE 'Populated Values' END
   WHEN 7 THEN IF ISNULL ([Happiness Score]) THEN 'Null Values' ELSE 'Populated Values' END
   WHEN 8 THEN IF ISNULL ([Health (Life Expectancy)]) THEN 'Null Values' ELSE 'Populated Values' END
   WHEN 9 THEN IF ISNULL ([Standard Error]) THEN 'Null Values' ELSE 'Populated Values' END
   END
   ```
10. Remove `Region` and `Country` from the `Rows` shelf.
11. Place `Null & Populated` on the `Rows` and `Color` shelves and `% Populated` on the `Columns` and `Label` shelves:
    ![](Images/B18435_02_04.png)
    Figure 2.4: Populated values
12. Change the colors to red for `Null Values` and green for `Populated Values` if desired. You can do so by clicking on `Color` in the `Marks` card and `Edit Colors`.
13. Click on the arrow in the upper-right corner of the `Select Field` parameter on your sheet and select `Single Value List`.
14. Select various choices in the `Select Field` parameter and note that some fields have a high percentage of null values. For example, in the following diagram, `32.98%` of records do not have a value for `Region`:
    ![](Images/B18435_02_05.png)
    Figure 2.5: Comparing null and populated values

Building on this exercise, let’s explore how we might clean and extrapolate data from existing data using the same dataset.

#### Extrapolating data

This exercise will expand on the previous exercise by cleaning existing data and populating some of the missing data from known information. We will assume that we know which country belongs to which region. We’ll use that knowledge to fix errors in the `Region` field and also to fill in the gaps using Tableau:

1. Starting from where the previous exercise ended, create a calculated field entitled `Region Extrapolated` with the following code block:
   ```
   CASE [Country]
   WHEN 'Afghanistan' THEN 'Southern Asia'
   WHEN 'Albania' THEN 'Central and Eastern Europe'
   WHEN 'Algeria' THEN 'Middle East and Northern Africa'
   WHEN 'Angola' THEN 'Sub-Saharan Africa'
   WHEN 'Argentina' THEN 'Latin America and Caribbean'
   WHEN 'Armenia' THEN 'Central and Eastern Europe'
   WHEN 'Australia' THEN 'Australia and New Zealand'
   WHEN 'Austria' THEN 'Western Europe'
   //complete the case statement with the remaining fields in the data set
   END
   ```
   As an alternative to a `CASE` statement, you could use an `IF` statement like:
   ```
   If [Country] = 'Afghanistan' then 'Southern Asia' 
   ELSEIF [Country] = 'Albania' then …
   END
   ```
   To speed up the tedious creation of a long calculated field, you could download the data to an Excel file and create the calculated field by concatenating the separate parts, as shown here:
   ![](Images/B18435_02_06.png)
   Figure 2.6: Compiling a calculation in Excel
