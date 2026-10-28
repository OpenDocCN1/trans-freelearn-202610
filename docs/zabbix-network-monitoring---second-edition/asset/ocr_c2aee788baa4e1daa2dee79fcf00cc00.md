As can be seen, global screens offer 19 different types of elements, while templated screens offer only seven.

For our screen right now, leave the Resource dropdown at Graph and click on Select next to the Graph field. Notice how the current template is selected and cannot be changed – all elements added to a templated screen must come from the same template. In the popup, click on CPU load & traffic in the NAME column, and then click on Add. Click on the + icon in the upper-right corner to add another column, and click on the Change link in the rightmost cell. In the Resource dropdown, choose Simple graph, click on Select next to the Item field, and then click on CPU load in the NAME column. Click on the Add button. Now, navigate to Configuration | Hosts and take a look at the available columns for each host. There is no column for screens. Templated or host screens are only configured on the template level; they do not get a copy on the host like items, triggers, and other entities do.

Let's go to Monitoring | Screens. If we look at the screen list there, the screen we just configured cannot be found. Templated or host screens can only be accessed from the host pop-up menu in the following locations:

+   - Monitoring | Dashboard (in the Last 20 issues widget)
+   - Monitoring | Overview (if hosts are located on the left-hand side)
+   - Monitoring | Latest data (if filtering by the Host field isn't done)
+   - Monitoring | Triggers
+   - Monitoring | Events
+   - Monitoring | Maps

They are also available from these two pages:

+   - Global search results
+   - The host inventory page