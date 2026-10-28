Open this graph in the monitoring section again:

![](bbox=[0.075, 0.112, 0.925, 0.33])

Sometimes, all that extra information can take up too much space, especially the legend if you have lots of items on a graph. For custom graphs, we may hide it. Re-enable these checkboxes in the graph configuration and save the changes by clicking on the same Update button.

# Graph item function

What we have now is quite similar to the simple graphs, though there's one notable difference when the displayed period is longer—simple graphs show three different lines, with the area between them filled, while our graph has single line only (the difference is easier to spot when the displayed period approaches 3 days). Can we duplicate that behavior? Go to Configuration | Templates and click on Graphs next to C_Template_Linux, then click on CPU load in the NAME column to open the editing form. Take a closer look at, FUNCTION dropdown in the Items section:

![](bbox=[0.45, 0.625, 0.548, 0.745])