
![](img/c69568baeee565fe02397867c05883e1_179_0.png)

Note that the data shown in the screenshot (C) relates from 2014 Q1 to 2015 Q4 (refer to the applied filters in the Date Range Filter (D) screen).

![](img/c69568baeee565fe02397867c05883e1_179_1.png)

## Adding a location page filter

This is the last page filter being added to the current page. This filter will allow you to confine your analysis to specific states or cities.

28. Once again, click on the Input Control icon on the Insert toolbar, and select the Location dimension:

![](img/c69568baeee565fe02397867c05883e1_180_0.png)

29. On the next screen, select All Members to include all states and cities to the filter:

![](img/c69568baeee565fe02397867c05883e1_180_1.png)

Adjust the filter, and drag to place it next to the Product filter (A). Deselect the All option, and click on California (B) to see this state's data on all the charts:

![](img/c69568baeee565fe02397867c05883e1_181_0.png)

Deselect California, and expand its node to see the list of cities in this state. Select Los Angeles (C) in the following screenshot, to analyze the data of this city:

![](img/c69568baeee565fe02397867c05883e1_181_1.png)

## Adding a story filter

By adding a story filter, you apply filters that affect every object on every page of your story. In this example, you will create a story filter to display data only for the California state. Since you have just one page in your story at the moment, you can revisit this exercise after completing all story pages in Chapter 7, *Working with Tables and Grids*.

30. On the toolbar, click on the Story Filter icon (A). An icon labeled Add Story Filter (B) will emerge just under the toolbar. Click on this icon, and select Location (C) from its menu. In the Set filters for Location dialog box, select the California state, (D) and click on OK to close the dialog box. The California state filter will be applied to the whole story. Later, if you wish to remove this filter, just click on the cross icon (E). All the story pages will be reinstated to their previous states, as shown in the following screenshot:

![](img/c69568baeee565fe02397867c05883e1_182_0.png)

## Creating a linked analysis

You saw how to restrict the scope of your analysis result by applying different types of filters on your story page objects. When you apply filters (for example, 2015 Q1 in the Date Range page filter), all other charts on the page are refreshed to display the data for the selected criteria. In addition to these filters, you can create linked analysis, using which you can create an interaction between charts within a story. When you create a linked analysis and drill through hierarchical data or create filters, multiple charts in your story are updated simultaneously.

> *The main difference between filters and linked analysis is that, in linked analysis, the filters are applied only to the charts you include in the analysis, whereas page and story filters affect all the charts.*

You can set up linked analysis for charts that belong to the same model. If the charts are based on different models, the models must contain linked dimensions. You will see an instance of a linked dimension in the next chapter. Now that we have a bunch of charts on our story page, we are in a position to create a linked analysis so that any actions performed on one chart (referred to as the main chart in this exercise) affect other linked charts in the story, but the actions performed on other charts have no effect on each other or on the main chart that stimulates the update. To apply the chart interaction, select the main chart. In this example, I selected the chart (illustrated next) that shows the gross margin percentage for each product by state.

31. On the Tools toolbar, click on Linked Analysis to open the Linked Analysis panel on the right side:

![](img/c69568baeee565fe02397867c05883e1_183_0.png)

You see three chart interaction options on the Linked Analysis panel, which are as follows:

- This chart (default option)(A): When you select this option, filtering and drilling through hierarchical data will only update the selected chart.
- Linked chart set (B): Filtering and drilling through hierarchical data on the selected chart will update all the charts in the set based on the same model or with linked dimensions to this chart's model. Choosing the filter on the datapoint selection option allows you to refine the setting so that, when you click a specific member in the main chart, other charts will be updated as well. If this option is not selected, other charts are affected only by drill or filter actions.
- Entire story (C): Filtering and drilling through hierarchical data on this chart will update all the charts in the story based on the same model or with linked dimensions to this chart's model.

By default, all the charts in your story are included in the linked chart set. To exclude a chart, select it, and deselect the Include this chart in set (D) option. The excluded charts will be moved from the Linked chart set area to the excluded charts area.

32. For now, just set the Chart interactions option to Linked chart set, and check Filter on datapoint selection. Keep all other values to their defaults, and click on Done. This will set interactions on the main chart to affect the others. Note that the chart interaction option for all other charts is set to the default This chart option. You can verify this in the Linked Analysis panel by selecting any other chart on the page and clicking on the Linked Analysis icon.

![](img/c69568baeee565fe02397867c05883e1_184_0.png)

In the main chart, just click on the Others product link (E), and see the effect of this action on other charts, as illustrated in the following two charts, which are linked to the main chart. Click on the others link again to revert to the previous state. You can also try the drill-down and filter options on the main chart to observe the effect of these actions on other charts. Now, perform the drill down and filtering operations on some other chart that is not set up for linked analysis. You will notice that the actions you perform on such a chart affect that particular chart only, and nothing happens to other charts on the page.

![](img/c69568baeee565fe02397867c05883e1_185_0.png)

## Adding dynamic text

You add headings and titles to your content to describe its purpose. In the same way, you add static and dynamic text to your story pages to explain the type of content these pages contain. In the current scenario, you will add dynamic text to your Story page to describe the type of content it renders.

33. Click on the Add icon on the Insert toolbar, which displays a menu. From this menu, select the Text option (A). A text box, as illustrated in the next screenshot, will be placed on your page with some default text. You can use the text box to display static heading and title information, such as Summary Report From Q1-2013 To Q4-2015. But what if you change the page data using date range filter by selecting some other duration? To cope with this situation, you can transform the static content of the same text box into a dynamic text. You will do this next! This transformation will change the page heading according to your selected filters.

![](img/c69568baeee565fe02397867c05883e1_186_0.png)

34. In the text box's toolbar, click on the More Action icon, and select Dynamic Text from the menu; see the following screenshot. Dynamic text is a text that is automatically updated based on the values from the source input control or filter.

![](img/c69568baeee565fe02397867c05883e1_186_1.png)

35. When you click on the Dynamic Text option in the menu, you see the Insert Dynamic Text dialog box. This dialog box displays five options in the left pane, and when you select an option in this pane, relevant options appear in the right pane. For this example, select the Input Controls option in the left pane to see the three filters you created in previous exercises. Select the checkbox for the Date filter to use this object as the source for your dynamic text:

![](img/c69568baeee565fe02397867c05883e1_187_0.png)

When you dismiss the Insert Dynamic Text box by clicking on the Create button, you see the text Q1(2013) - Q4(2015) in the text box. Yours might be different depending upon the date filters you have applied in the Date Range Filter screen.

36. Click on the text box, and type Summary in front of the dynamic date text. Click on the More Actions icon (A), and select Edit Styling (B) from the menu. Change the Color (C) and Style (D) of the text, as illustrated in the following screenshot:

![](img/c69568baeee565fe02397867c05883e1_187_1.png)

In the Date Range Filter screen, select 2015 in the first list, Q2 in the second, again 2015 in the third, and Q4 in the last one. The dynamic text will be updated accordingly to show the selected period, as shown in the following illustration:

![](img/c69568baeee565fe02397867c05883e1_187_2.png)
