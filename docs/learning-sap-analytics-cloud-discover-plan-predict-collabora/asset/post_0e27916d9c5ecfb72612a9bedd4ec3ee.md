
![](img/c69568baeee565fe02397867c05883e1_152_0.png)

The chart will be updated with two measures: Original Sales Price (A) and Incremented Sales (B). Click on Incremental Factor Input Control (C) to expand the list. Select a value in the list, and see its impact on the Incremental Sales bars (D). As you can see from the first view, the sale value for Carbonated Drinks (172.82) displays an incremented figure of 432.04 (D) when the incremental factor is set to 2.5:

![](img/c69568baeee565fe02397867c05883e1_152_1.png)

In the second view, the same sales figure is incremented five times and shows 864.08 (E):

![](img/c69568baeee565fe02397867c05883e1_153_0.png)

The third view shows the incremented effect when the volume of sales is increased by 10. If you wish to modify the list of values, click on the Edit Input Control link (F):

![](img/c69568baeee565fe02397867c05883e1_153_1.png)

## Creating a Restricted Measure

The Restricted Measure option acts like a filter. When you create this measure, you restrict your results to some specific measures. In this exercise, you will create a Restricted Measure to confine your analysis to a specific state:

78. Click on the Add Measure link in the Builder tab, and select Create a New Calculation. This time, select Restricted Measure for the calculation type.
79. Type `California` for the Name measure.
80. Select Original Sales Price for Measure.
81. Select Location for Dimensions. Members of the Location dimension will appear in the next screen, where you will select a member to restrict your results.

82. From the Values or Input Controls drop-down list, choose the first option, Select by Member. The value(s) you select on the subsequent screen will appear on this list (A):

![](img/c69568baeee565fe02397867c05883e1_154_0.png)

83. When you click on the Select by Member option in the previous dialog box, you see this one. Place a check mark in front of California to select this member from the Location dimension, and click on OK. The selected member from the Location dimension will appear in the Values or Input Controls List in the Calculation Editor interface, and the same member will be added to the chart as a Restricted Measure (B) option:

## Select Values for Location

Now you can see three values for each product category--original sales, incremented sales, and actual sales reported in California:

![](img/c69568baeee565fe02397867c05883e1_155_0.png)

![](img/c69568baeee565fe02397867c05883e1_155_1.png)

## Creating the Difference From measure

The third type of calculation you can create in the Calculation Editor interface is Difference From. You create this type of calculation to compare values between time periods. In a previous exercise, you created a Bar chart to show the gross margin percentage for each product by state. Here, you will use the same chart to create a Difference From calculation in the Calculation Editor interface to evaluate the difference in gross margin percentage between 2014 and 2015.

To create a new chart from an existing one, you have three options: Copy, Copy To, and Duplicate. If you select an existing chart and select the Copy option (A), or press Ctrl+C, the chart is placed on the clipboard. Click on the Paste option (B) or press Ctrl+V to recover the clone from the clipboard on your current page. The Copy To option (C) has some sub-options that let you copy the selected object to a new canvas or to another page in the story. In the current scenario, you will use the Duplicate option to create a clone of the selected chart.

84. Click on the chart (as shown in the following screenshot), and select Duplicate from the Copy and Paste menu in the toolbar. This action will create a duplicate of the selected chart on the same page:

![](img/c69568baeee565fe02397867c05883e1_156_0.png)

Select the duplicate chart, and open the Designer panel.

85. On the Builder tab, click on the remove icon under the Measures section to remove the Gross Margin % measure.

86. After removing the existing measure, click on the Add Measure link, and select Click to Create a New Calculation to create the new Difference From type calculation measure in the Calculation Editor interface:

![](img/c69568baeee565fe02397867c05883e1_157_0.png)

87. In the Calculation Editor interface, select Difference From for the Type of calculation.

88. Type GM% Change Between 2014-2015 in the Name box.

89. In the Measure section, choose Gross Margin. This is the measure on which the calculation will be based. The Time Dimension is already set to the date dimension.

90. For Current Value or Input Control, choose Select by Member, and on the ensuing screen, expand the all node to select 2015. Click on OK to close the Select Values for the Date dialog box. The selected year (2015) will appear in the Current Value or Input Control box.

91. Select previous from the Difference From drop-down list; the other option is Next. Here, you can specify a previous period or a later period to compare the time period you selected in Current Value.

92. In the Nth Period section, you provide a number other than the current or baseline time period that you want to include in the calculation. The value you provide here uses the level of time granularity you chose in the Current Value section. So, in the current scenario, one period is a year. Enter 1 in the Nth Period section to compare the results from 2015 to those from 2014.

93. Check the Calculate as Percentage checkbox to see the results as a percentage.

94. When you check the Calculate as Percentage option, you also need to specify whether to divide by the Current Value or the Nth Period Value. In this case, select Nth Period Value to divide the results from 2015 by those for 2014. After providing all these parameters, click on OK to close the Calculation Editor interface:

![](img/c69568baeee565fe02397867c05883e1_157_1.png)

As you can see, the chart now displays the percentage change in the gross margin for each product category, for each state:

![](img/c69568baeee565fe02397867c05883e1_158_0.png)

## Summary

A story is the most significant component of SAP Analytics Cloud, where you visually analyze your data using different types of objects. In this chapter, you got hands-on exposure to this imperative subject and learned how to populate your story canvases by adding and configuring different types of charts. Here's a brief summary of the topics you went through in this chapter:

- An introduction to and creation of a story
- A brief about the two demo models that you will also utilize in subsequent chapters
- Using Numeric Point charts to display summarized information in bold-faced dynamic numbers
- Learning how to use the Calculation Editor interface to create your own formulas
- Adding thresholds to your charts to present the status of values in different colors
- Styling your charts with the help of options provided in the styling tab
- Using Numeric Point, Line, Bar, and Stacked Bar/Column charts to present data visually
- Applying filters to time dimension to present data for specific years
- Drilling up and down in charts to observe the data at different levels
- Creating a Calculated Measure with Input Controls and seeing how to confine your analysis by adding Restricted Measures.
- Finally, you learned how to create a measure using the Difference From calculation type to compare values between time periods.

In the next chapter, you will learn some more important techniques to present the data using key performance indicators (KPIs) in your story page and adding filters to your page to see specific data. You will also learn how to add a reference line to a chart, add static and dynamic text, shapes, images, hyperlink, and you will be guided to save a story.

## Extending Stories with KPI, Filters, and Other Handy Objects

In the previous chapter, you created different types of chart to present data graphically. You will continue your efforts on the same story page in this chapter as well. Here, you will create a bullet chart, which is usually added to a story page to monitor an organization's performance via key performance indicators (KPI). You will also learn how to add page filters to narrow down the scope of your analysis. The two segments are illustrated in the following screenshot.

The bullet chart (A) will show the status of the gross margin for each state. Date Range Filter (B) is a page filter that is added to limit the results of all page charts to the selected period(s).

Similarly, the Product (C) and Location (D) page filters will allow you to see the data for specific products and locations.

![](img/c69568baeee565fe02397867c05883e1_160_0.png)

In addition to the aforementioned two story features, you will learn the following:

- Adding a reference line to a chart
- Creating a linked analysis
