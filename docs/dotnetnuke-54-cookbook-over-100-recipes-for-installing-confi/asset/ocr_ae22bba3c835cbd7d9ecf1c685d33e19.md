![](bbox=[0.21, 0.083, 0.791, 0.248])

+   6. Click on the **Browse** button and select a sample image.
+   7. Click on **Upload File** to upload the image. Repeat for each image you have.

![](bbox=[0.106, 0.332, 0.894, 0.527])

+   8. With the images available to the site, we put the code in our module to display the images when we display employees.
+   9. Launch the Development Tool and load the **Employee** project.
+   10. Double-click to open the `ViewEmployee.ascx` file.
+   11. Locate the Datagrid and add a new column just after the **Edit** and **Delete** links:

```
```aspx
<columns>
    <dnn:imagecommandcolumn CommandName="Edit" ImageUrl="~/images/edit.gif" EditMode="URL" KeyField="ItemID" />
    <dnn:imagecommandcolumn CommandName="Delete" ImageUrl="~/images/delete.gif" KeyField="ItemID" />

    <asp:TemplateColumn HeaderText="Icon">
        <itemtemplate>

            <asp:HyperLink id="imgView"
                ImageUrl='<%# DataBinder.Eval
                    (Container.DataItem,"ImageURL") %>'>
```
```