
## Creating a dashboard

2. In the `Dashboard` pane, located on the left side of Figure 1.17, double-click on each of the following worksheets (in the order in which they are listed) to add them to the dashboard pane: `Count of Disciplines`, `Attendees`, `Medals`, and `Medal Shapes`:

![](Images/B18435_01_17.png)

Figure 1.17: Creating a dashboard

1. In the bottom-right corner of the dashboard, click in the blank area (indicated by an arrow in Figure 1.18) to select a `container`. Containers are a selection of sheets that auto-adjust to fit next to/underneath each other on your dashboard:

![](Images/B18435_01_18.png)

Figure 1.18: Deleting a container

1. After clicking in the blank area, you should see a blue border around the filter and the legends. This indicates that the vertical container is selected. Select the vertical container handle and delete it by clicking the `X`.

2. A selected container can also be dragged to a different location on the dashboard, replacing an existing sheet or sharing space. Note the gray shading in Figure 1.19, which communicates where the selection will be placed. Drag the `Attendees` worksheet by selecting and holding the handle and put it on top of the `Count of Disciplines` worksheet in order to swap the two:

![](Images/B18435_01_19.png)

Figure 1.19: Moving worksheets

The gray shading provided by Tableau when dragging elements, such as worksheets and containers, onto a dashboard helpfully communicates where the elements will be placed. Take your time and observe carefully when placing an element on a dashboard or the results may be unexpected.

1. Note the `Floating` and `Tiled` buttons at the bottom left, next to your dashboard, as well as under the handle dropdown. If you select `Floating` instead of `Tiled`, your worksheets will not auto-adjust anymore and will be “floating” around the dashboard instead. This is a free form of worksheet arrangement on a dashboard, which is a powerful functionality, especially combined with transparent backgrounds. Note that floating can also be achieved by selecting it from the `More Options` dropdown on the right-hand side of every sheet in the dashboard:

![](Images/B18435_01_20.png)

Figure 1.20: Tiled and Floating

1. Now, you can format the dashboard as desired. The following tips may prove helpful:

- Adjust the sizes of the elements on the screen by hovering over the edges between each element and then clicking and dragging. Drag the edges of containers to adjust the size of each worksheet.
- Make an element floating, as shown in Figure 1.18.
- Create horizontal and vertical containers by dragging `Horizontal` and `Vertical` from the `Objects` pane onto the dashboard. Other objects, such as `Text`, `Image`, and `Blank`, can be added to containers too.
- Display the dashboard title by selecting `Dashboard` in the top pane, then `Show Title`. Double-click the title itself to adjust:

![](Images/B18435_01_21.png)

Figure 1.21: The Olympics dashboard

As you can see, you have just created your first dashboard. All worksheets are in place now. In the next part, we will add filter functionality to make the dashboard more interactive and meaningful.

### Adding interactivity to a dashboard

One of the primary benefits of Tableau is the interactivity it provides for the end user. Dashboards aren’t simply for viewing; they’re meant for interaction. In this exercise, we’ll add interactivity to the dashboard that was created in the previous exercise:

1. Select the `Medals` sheet on the dashboard and click on the drop-down arrow on the right-hand side, which will open a menu as shown in Figure 1.22. From there, select `Filters`, then `Sport`:

![](Images/B18435_01_22.png)

Figure 1.22: Adding a filter

1. Now select the newly created filter, `Sport`, click again on the drop-down options arrow, and select `Multiple Values (dropdown)`, as well as `Apply to Worksheets | All Using This Data Source`, as shown in Figure 1.23:

![](Images/B18435_01_23.png)

Figure 1.23: Filter settings

1. Lastly, place the filter above the `Medals` sheet by clicking and dragging it.

2. To use the images of the medals as a filter for the other worksheets on the dashboard pane, click the `Use as Filter` icon located at the top-right corner of the `Medals Shapes` worksheet:

![](Images/B18435_01_24.png)

Figure 1.24: The Use as Filter option

1. Alternatively, navigate to `Dashboard | Actions…`. In the dialog box, click `Add Action | Filter` and create a filter, as shown:

![](Images/B18435_01_25.png)

Figure 1.25: Navigating to filters

![](Images/B18435_01_26.png)

Figure 1.26: Adding a filter action

From here on in, you are good to go and use filters and action filters.

In Tableau, filters are used to subset the underlying data based on certain conditions, while action filters allow users to interactively filter data by clicking on a visual element.

In Chapter 13, Improving Performance, this topic will be discussed in more detail.

Having completed the preceding dashboard exercise, you should now be able to click on various objects on the dashboard to observe the interactivity.

To learn some more advanced dashboard techniques, be sure to check out Chapter 11, Designing Dashboards and Best Practices for Visualizations.

To conclude, you have learned how to put existing worksheets, as tiled or floating objects, on a dashboard. You have changed the dashboard layout by dragging and dropping, as well as by using containers, filters, and action filters. Each of these core activities will be repeated multiple times throughout the book, so feel free to come back to this chapter and repeat the exercise steps whenever needed!

Next, you will learn how to connect your own data to Tableau and work with it.

# Connecting Tableau to your data

At the time of writing, Tableau’s data connection menu includes more than 90 different connection types. And that’s somewhat of an understatement since some of those types contain multiple options. For example, `Other Files` includes more than 30 options. Of course, we won’t cover the details for every connection type, but we will cover the basics. The official documentation on all connectors can be found here: [https://help.tableau.com/current/pro/desktop/en-us/exampleconnections_overview.htm](https://help.tableau.com/current/pro/desktop/en-us/exampleconnections_overview.htm).

Upon opening a new instance of Tableau Desktop from within Tableau Desktop (`File | New`), you’ll notice a `Connect to Data` link in the top-left corner of the workspace. Clicking on that link will enable you to connect to the data. Opening a new instance of Tableau from the application’s location on your computer will bring up the blue `Connect` pane right away. Alternatively, you can click on the `New Data Source` icon on the toolbar.

Although in future chapters we’ll connect to other data sources, here we’ll limit the discussion to connecting to Microsoft Excel and text files.

## Connecting to a file

Let’s see how you can connect to a file, using Excel as an example:

1. In Tableau, navigate to `Data | New Data Source | Excel` to connect to the `Sample - Superstore` dataset that is installed with Tableau Desktop (it should be located on your hard drive under `My Tableau Repository | Data sources`).

2. Double-click on the `Orders` sheet.

3. Click on the `New Worksheet` tab, as shown in Figure 1.27:

![](Images/B18435_01_27.png)

Figure 1.27: New worksheet

1. Rename the newly created tab to `First Worksheet` by right-clicking and selecting `Rename`.

2. Place `Discount` on the `Text` shelf in the `Marks` card.

3. Double-click on `Profit` and `Sales`:

![](Images/B18435_01_28.png)

Figure 1.28: First worksheet

You’ve just created your first worksheet!
