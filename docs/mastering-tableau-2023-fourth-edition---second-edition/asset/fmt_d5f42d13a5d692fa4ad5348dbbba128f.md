
If you set partitioning and addressing for a given table calculation and then add dimensions to the view, usually, Tableau will not automatically adjust the partitioning and addressing settings; they are locked down. However, when using `Table (down)`, `Pane (across)`, and the like, Tableau will make automatic adjustments as dimensions are added to the view.

This leads us to our next guideline:

Place all needed dimensions on the desired shelves *before* setting partitioning and addressing for table calculations.

Following these guidelines will help ensure that you are always clear about how your table calculations are being applied. It might take longer in the first place, but ultimately, you’ll be more in control of table calculations and can develop faster.

## Building a playground

Let us set up a simple playground environment to quickly and efficiently explore partitioning and addressing:

1.  In the workbook associated with this chapter, navigate to the `Playground` worksheet.

2.  Place `Category` on the `Rows` shelf and the `Index` calculation on the `Label` shelf.

3.  The `Index` calculation is simply `Index()`. Click on the drop-down menu associated with `Index` and select `Edit Table Calculation`. In the resulting dialog box, click `Specific Dimensions`.

4.  Position the screen components optimally. See the following screenshot for one possible setup:

![](Images/B18435_05_17.png)

Figure 5.17: Building a playground

You can see in *Figure 5.17* that we created a worksheet displaying the three categories `Furniture`, `Office Supplies`, and `Technology`. By adding the `Index` field to the `Text` shelf, we have the minimum requirement to use a table calculation, which is one dimension and one table calculation. Now we can change the compute used in the table calculation and can spot what changes in the visualization. Later on, we will explore more complex examples; however, always start easy and increase the level of complexity slowly to fully understand what is happening.

In the following pages, we will utilize our playground extensively and modify it as necessary. However, for the sake of efficiency, we will need to keep the focus on the playground and the accompanying discussion narrow. The discussion will be confined to dimensions on the `Rows` and `Columns` shelves and the `INDEX` function on the `Text` shelf.

We could explore different functions on various shelves and the different options that they afford. For instance, placing a date field on the `Pages` shelf will cause a table calculation that uses the `TOTAL` function to display an option to compute the total across all pages. Regretfully, exploring every possible nuance is simply not possible in one chapter, but if you are interested in this topic, feel free to check out the Tableau help page for more content and examples: `https://help.tableau.com/current/pro/desktop/en-us/functions_functions_tablecalculation.htm`.

## Partitioning and addressing with one dimension

Let us use our playground to start exploring partitioning and addressing with the simplest possible example:

![](Images/B18435_05_18.png)

Figure 5.18: Compute Using | Category

In this simple example, addressing `Category` causes each member of the `Category` dimension to be counted. This demonstrates that addressing a dimension determines the direction of the calculation. In our example, we have the `Table Calculation` `Index`, but `Index` depends on partitioning and addressing as we learned before. `Category` serves as a partition in the `Rows` shelf. We split the data into three: `Furniture`, `Office Supplies`, and `Technology`. Now we also say address `Category` for the `Table Calculation`. We did that by selecting `Category` from `Compute Using` (*Figure 5.18*). Now the `Table Calculation` will be applied to the categories and hence counts 1, 2, 3 categories.

If you were to select `Compute Using` | `Cell`, the partition into three categories would still remain in the view—you would still see `Furniture`, `Office Supplies`, and `Technology`—but the counting would happen per cell and not for the whole `Category` dimension. Hence, the `Index` will count 1, then move to the next cell and count 1, then the next cell and again count 1:

![](Images/B18435_05_19.png)

Figure 5.19: Compute Using | Cell

## Partitioning and addressing with two dimensions

Two additional options are made available when partitioning and addressing two or more dimensions: `At the level` and `Restarting every`. You can compare this scenario to the task of counting all countries per continent. In this scenario, `At the level` will be countries since you are not counting streets or trees but countries. `Restarting` will be continents. After you are done counting countries for one continent, you start at 1 again for the next continent:

![](Images/B18435_05_20.png)

Figure 5.20: Two dimensions

Both `At the level` and `Restarting every` allow the author to choose dimensions from a drop-down menu. `At the level` allows the author to choose what level to increment at, and as the name suggests, `Restarting every` allows the author to choose which dimensions to restart on. The examples here will provide context for your understanding.

Note that `At the level` has one additional choice: `Deepest`. In this case, setting `At the level` to `Deepest` is the same as selecting `Ship Mode`. This leads us to our next guideline:

It is not necessary to choose the bottom dimension in the `At the level` drop-down menu. It is always identical to `Deepest`.

To recreate the iterations listed here, you will need to make some changes to the playground environment. In addition to `Category` on the `Rows` shelf and `Index` on the `Label` shelf, also place `Ship Mode` on the `Rows` shelf. We will not cover iterations that include one or more dimensions on the `Columns` shelf since the behavior of these possibilities is much the same.

As you consider and/or reproduce the following options, note that the `Addressing` order is important. For examples 3-5, `Category` is first on the addressing list. For options 6-8, `Ship Mode` is first on the list.

In the first example, configure the partitioning and addressing settings thus:

*   `Partitioning`: `Category`
*   `Addressing`: `Ship Mode`
*   `At the level`: -
*   `Restarting every`: -

This will produce the following visualization:

![](Images/B18435_05_21.png)

Figure 5.21: Example 1

Here, we count the `Ship Mode` per `Category`.

In the second example, configure the partitioning and addressing settings thus:

*   `Partitioning`: `Ship Mode`
*   `Addressing`: `Category`
*   `At the level`: -
*   `Restarting every`: -

This will produce the following visualization:

![](Images/B18435_05_22.png)

Figure 5.22: Example 2

In this example, we count the `Category` per `Ship Mode`.

In the third example, configure the partitioning and addressing settings thus:

*   `Partitioning`: -
*   `Addressing`: `Category`, `Ship Mode`
*   `At the level`: `Deepest`
*   `Restarting every`: `None`

This will produce the following visualization:

![](Images/B18435_05_23.png)

Figure 5.23: Example 3

This time, we count the `Category` and `Ship Mode` combination.

In the fourth example, configure the partitioning and addressing settings thus:

*   `Partitioning`: -
*   `Addressing`: `Category`, `Ship Mode`
*   `At the level`: `Deepest`
*   `Restarting every`: `Category`

This will produce the following visualization:

![](Images/B18435_05_24.png)

Figure 5.24: Example 4
