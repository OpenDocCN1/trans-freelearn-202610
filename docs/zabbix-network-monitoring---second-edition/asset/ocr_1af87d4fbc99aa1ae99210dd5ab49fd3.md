# Visualizing Data with Screens and Slideshows

Wonderful! Elements marked as dynamic now show data from the selected host, while non-dynamic elements show the same data no matter which host is selected. The static elements could be maps like in our screen, but they could also be graphs if the **Dynamic item** option hasn't been checked for them. That would allow us to switch a screen to show server information in some graphs, but other graphs could keep on showing general network information.

> Only graphs from hosts can be added to screens; graphs from templates cannot. For a dynamic screen item, there is a risk that the host from which the graph was initially selected gets deleted, thus breaking the screen. Old versions of Zabbix allowed us to include graphs from templates here, and that functionality might return later.

# Additional screen elements

This is a nice, simple screen, but there were many more available screen elements to choose from, so let's create another screen. Go to the list of screens—if a screen is shown in the monitoring view, click on **All screens**, and then click on the **Create screen** button. In the resulting form, enter "Experimental screen" in the **Name** field, enter 2 for both the **Columns** and **Rows** fields, and then click on **Add**. In the screen list, click on **Constructor** next to **Experimental screen**. As before, click on the **Change** link in the upper-left cell. In the **Resource** dropdown, choose **Simple graph**, and then click on **Select** next to the **Item** field. Select **A test host** from the **Host** dropdown.

As we can see, all the simple graphs that are available without any manual configuration can also be added to a screen. Here, click on the **CPU load** entry. In the **Width** field, enter 600, and then click on **Add**. Click on the **Change** link in the upper-right cell. Choose **History of events** from the **Resource** dropdown, and then click on **Add**.

Well, suddenly our graph doesn't look that great any more—it should be taller to fit this layout. We could place it below the events list, but that would require deleting it and reconfiguring the lower-right cell. Well, not quite. Drag the graph to the lower-right cell and release the mouse button: