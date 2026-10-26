# There's more...

A very important part of this recipe is found when we are adding the form to the manifest. For a secondary view control to work it must have a unique key that we'll use to identify the form when we navigate to it. In this example, we gave the new form the key AcmeEmployeeDisplay when we defined it in the manifest with the <key>AcmeEmployeeDisplay</key> element.

# Controlling navigation with NavigateURL

In this recipe we'll see how to control navigation within a module using NavigateURL. In this example, we will hook up the Datagrid of employees so that when the link for an employee is clicked, navigation will pass to the secondary View control created in the previous recipe and display the employee details.

> Note: this technique is for navigation within a module, not outside to other modules or pages.

# Getting ready

To follow along with this recipe you must have completed the following recipe:

+   - Creating a secondary View control

# How to do it...

+   1. Start by launching the Development Tool and loading the Employee project.
+   2. The first step is to add a link to the Datagrid of employees that will go to the secondary View control. Double-click to open the ViewEmployee.ascx file.
+   3. We add a new column in the Datagrid with the following code:
+   ```
+   <dnn:imagecommandcolumn CommandName="Display" ImageUrl="~/images/view.gif" EditMode="URL" KeyField="ItemID" />
+   <dnn:imagecommandcolumn CommandName="Edit" ImageUrl="~/images/edit.gif" EditMode="URL" KeyField="ItemID" />
+   <dnn:imagecommandcolumn CommandName="Delete" ImageUrl="~/images/delete.gif" KeyField="ItemID" />
+   ```
+   4. The next step is to modify the Page_Init routine so that the correct navigate URL is put in the Datagrid when it is populated. Double-click to open the ViewEmployee.ascx.vb file.