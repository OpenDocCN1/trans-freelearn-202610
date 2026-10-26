+   8. To import the CSV file back into your Form and List module, the process is very similar. Start by selecting **Import from CSV File** from the module action menu.

![](bbox=[0.237, 0.134, 0.762, 0.209])

+   9. Select the folder where you saved the CSV file and choose comma (,) for the delimiter.
+   10. Click on **Import** to load the file.

> Because the export file in this example includes the unique identifier, it will not import records that already exist in your custom table. To see the import work you may need to delete some records from the table before trying the import.

# # Creating custom module actions

In many recipes we have used the module action menu that appears next to the module title and is used to select additional module features. Modules that inherit from the `PortalModuleBase` class receive several default actions in the menu (things like Import/Export Content). In this recipe we will show how to add your own actions in the module menu using the `iActionable` interface.

There are three basic tasks to define our own module actions:

+   - Define the custom action menu item
+   - Create a subroutine to handle the `Click` event
+   - Connect the `Click` event handler to the menu item

# ## Getting ready

To follow along with this recipe you must have completed the following recipe from *Chapter 5, Building, Debugging, and Deploying Modules:*

+   - *Deploying a module as a standalone package*

For this recipe we will demonstrate the action menu processing by creating a simple HTML table of statistics on the View Employee form that is hidden until we click on the **Show Statistics** action menu item.