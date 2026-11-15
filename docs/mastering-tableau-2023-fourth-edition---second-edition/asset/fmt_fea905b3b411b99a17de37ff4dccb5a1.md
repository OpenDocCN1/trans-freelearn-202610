
Prep comes with lots of different features. Sometimes, you might use many different tools to prepare your dataset in order to get it in the shape you desire. Other times, you might just run an aggregation (one feature) and be done. It really depends on the dataset itself and the expected output. The fact is, the closer your Prep output data is to what you need for your Tableau Desktop visualization, the more efficiently VizQL will run on Tableau Desktop. Fewer queries in Tableau Desktop means faster generation of dashboards.

To me, the best part about Prep is that it can handle a huge amount of data. Sometimes, I even use it for datasets I don’t want to visualize in Tableau Desktop, just to get a quick overview of, for example, how many rows contain a specific word, how many columns are needed, what happens to the date range if I filter a particular value, and so on! Within a few minutes, I have insights that would have taken me much more time to get with database queries or Excel functions. I hope that by the end of this chapter, you will be able to cut your time spent data prepping in half (at least). We will divide the prepping features into five subcategories: cleaning, unions and joins, aggregating, pivoting, and scripting. Let’s start with cleaning data!

## Cleaning data

We have seen the following canvas before in the *The Prep GUI* section. To create the cleaning step, the user can simply click on `+` next to the input and select `Add: Clean Step`. During the cleaning step, multiple operations can be performed, such as filtering or creating a calculated field. Also note the recommendations Prep gives you:

![](Images/B18435_03_09.png)

Figure 3.9: Recommendations

Prep analyzes the column content and proposes changes that might fit the data. The column `listing_url`, for example, is being recognized as a web page and therefore Prep recommends you change it to the data role URL. The second, third, and several more recommendations after `listing_url` are to remove certain columns. This is probably the case because the column does not contain any data or contains only a small amount of data. The list goes on.

This feature can be very useful, especially for unfamiliar datasets. My way of working would be to look at the recommendations, check whether they make sense, and execute the change—or not. Don’t blindly trust these recommendations, but they can point out data flaws you might have missed otherwise.

Data is often messy, involving null values, typos from manual entries, different formatting, changes in another system, and so on. As a consequence, you will have to sort out the mess before you can get reliable results from an analysis or a dashboard. This section will show you how to clean data at the column level.

Once a value is selected within your clean step, you have the option to `Keep Only`, `Exclude`, `Edit Value`, or `Replace with Null`:

![](Images/B18435_03_10.png)

Figure 3.10: Quick access

None of these changes will change the data source itself. Prep is like an in-between step, or a filter between the original data source and your Tableau Desktop. Excluding a value, using the option you can see in Figure 3.10, will only remove it from Prep. However, if used later on as input for Tableau Desktop, there won’t be an option to add that specific value back in. This option will remain in Prep only.

Another slightly hidden option is to click on the ellipses `...` next to the column headers (as shown in *Figure 3.11*) and select `Clean`:

![](Images/B18435_03_11.png)

Figure 3.11: More options

This `Clean` functionality operates based on the data type of the column. In the preceding screenshot, the data type is a string (indicated by the `Abc` icon). For other data types, the option will be grayed out since `Clean` is only available for strings. The option allows you to make use of eight more cleaning features:

*   Make Uppercase
*   Make Lowercase
*   Remove Letters
*   Remove Numbers
*   Remove Punctuation
*   Trim Spaces
*   Remove Extra Spaces
*   Remove All Spaces

The data type can be changed just above each column header; you will find a symbol above the column name, which can be changed by clicking on it, just like in Tableau Desktop:

![](Images/B18435_03_12.png)

Figure 3.12: Changing Data Type

This comes in handy in case Prep misinterprets the data type of a column. A wrongly assigned data type can have an effect on the calculation you perform on it and how Tableau Desktop would visualize the column.

In the upcoming section, we will practice combining multiple datasets. You might want to combine, for example, order data with shipment data (using a join) or append 2020 sales data to your 2019 sales dataset (using a union). To find out how, read on!

## Unions and joins

We will now join the three datasets, so that we may analyze and visualize them together. The nature of the three datasets (`listings.csv`, `reviews.csv`, and `calendar.csv`) requires a join but not a union. However, I will still walk you through the steps of a union in order for you to know when you need it!

### Adding unions

A union in Tableau is the appending of data to an existing dataset. Imagine you have two Excel files. Both have the exact same header, but one contains data from 2019, and the other data from 2020. If you union the 2020 data to the 2019 data, you append the rows of the 2020 files to add them underneath the rows of the 2019 files. To perform a union, both datasets need to have almost the same layout/header. Why almost? You will see in the following exercise:

1.  To begin this exercise, your Prep flow pane should look like this:

    ![](Images/B18435_03_13.png)

    Figure 3.13: Flow pane

2.  Add a clean step by clicking `+` next to the `listings` dataset.

    You can change the color of each step according to your preferences by right-clicking on a step and selecting `Edit Color`.

3.  Select the column `host_identity_verified` and filter on `True`:

    ![](Images/B18435_03_14.png)

    Figure 3.14: Filtering on True

4.  Now, create a calculated field, called `DatasetA_ID`, containing just a string, `'A'`:

    ![](Images/B18435_03_15.png)

    Figure 3.15: Creating a calculated field

5.  Create a second cleaning step from the same listings data and filter this time on `True` in the `host_identity_verified` column:

    ![](Images/B18435_03_16.png)

    Figure 3.16: Filtering on True

6.  Create a calculated field, `DatasetB_ID`, containing just a string, `'B'`, using the same process shown in *Figure 3.15*. Renaming the steps by right-clicking and hitting `Rename` helps to keep track of the applied changes. Your flow should look like the following:

    ![](Images/B18435_03_17.png)

    Figure 3.17: Visual flow

7.  You have now created two datasets that can be combined using a union, without changing the original data. Union the two by either selecting `Union` after clicking on `+` next to `Dataset A` or by dragging one step over the other (`Dataset B` over `Dataset A` or vice versa):

    ![](Images/B18435_03_18.png)

    Figure 3.18: Union

8.  Check whether all fields match by clicking on `Union 1` and looking at the input details at the bottom left of the screen:

    ![](Images/B18435_03_19.png)

    Figure 3.19: Visual check

In the preceding screenshot, you can see that on the left, you have information about the union. From top to bottom: the `Inputs` and their respective colors (these are the same as in the flow pane), the `Resulting Fields`, showing 2 mismatching fields, and finally, the `Mismatched Fields` themselves: `DatasetA_ID` and `DatasetB_ID`. The colors next to each mismatched field show that our `DatasetB_ID` field does not exist in `DatasetA`, and the `DatasetA_ID` field does not exist in `DatasetB`.
