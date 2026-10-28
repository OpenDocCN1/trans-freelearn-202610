![](bbox=[0.129, 0.091, 0.864, 0.14])

It really is better, we got rid of the huge empty area. Pie graphs could also be useful for displaying memory information—the whole pie could be split into buffers, cached, and actual used memory, laid on top of the total amount of memory. In such a case, total memory would get a type set to **Graph sum**, but for all other items, **TYPE** would be set to **Simple**.

Let's try another change. Edit the **Used diskspace (pie)** graph again. Select **Exploded** in the Graph type dropdown and mark the checkbox next to 3D view. Save these changes and refresh the graph view in **Monitoring | Graphs**:

![](bbox=[0.204, 0.346, 0.796, 0.645])

Remember the "function" we were setting for the normal graph? We changed between **avg** and **all**, and there were also **min** and **max** options available. Such a parameter is available for pie graphs as well, but it has slightly different values:

![](bbox=[0.457, 0.746, 0.542, 0.85])