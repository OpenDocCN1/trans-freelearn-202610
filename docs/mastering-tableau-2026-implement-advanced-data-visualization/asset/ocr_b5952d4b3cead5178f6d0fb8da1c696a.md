+   • Continuous and discrete aren't synonymous with dimensions and measures:

+   • It's common for new Tableau authors to conflate continuous with measures and discrete with dimensions. They aren't synonymous. A measure may be either discrete or continuous.

+   • Also, a dimension, if it's a number, may be discrete or continuous. To prove this point, right-click on any numeric or date field in Tableau and note that you can convert it:

![](bbox=[0.17, 0.19, 0.86, 0.68])

Figure 1.45: Converting between discrete and continuous

+   • Discrete values can be sorted. Continuous values can't: Sortable/not sortable behavior is most easily observed with dates and a measure like Sum(Sales), as shown in the following example:

![](bbox=[0.35, 0.775, 0.65, 0.865])

Figure 1.46: Left: continuous (not sortable); right: discrete (sortable)