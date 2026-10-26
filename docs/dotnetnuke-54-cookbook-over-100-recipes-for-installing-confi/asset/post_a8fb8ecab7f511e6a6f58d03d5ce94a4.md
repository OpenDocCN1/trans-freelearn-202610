
## Cool Web Controls

In this chapter we will cover the following topics:

- Adding web controls to your Toolbox
- Showing an e-mail link in a DataGrid
- Showing checkboxes in a DataGrid
- Showing a thumbnail image in a DataGrid
- Creating labels you can edit
- Suggest text while typing
- Showing data in a Treeview
- Using a Tabstrip to separate content
- Using a CAPTCHA control for security
- Creating a multi-state checkbox

### Introduction

One of the powerful features of DNN is the variety of flexible and reusable controls that are available for custom module development. These include many of the web controls seen on the core DNN pages, controls such as editable labels, tree views, CAPTCHA controls, and Tab controls. In this chapter, we will see how to add these web controls to custom modules and tie them to the tables in the database.

In general, using these controls requires four simple steps:

- Adding the control code to the View or Edit `.ascx` file
- Adding a new property to the info object that will supply the values for the control
- Binding the control to the values
- Capturing the value from the control and saving to the database (if Edit page)

As the recipes in this chapter focus on web controls and values from the database, it is necessary that you have built and deployed the sample Employee module from *Chapter 6*. We will also extend the DataGrid from *Chapter 6* and show the different kinds of columns it can display.

### Adding web controls to your Toolbox

If you frequently use the visual editor in the development tool to layout your pages, this short recipe will show you how to add the DNN web controls to the Toolbox.

### How to do it...

1. Launch the Development Tool.
2. Change the editor to Design mode.
3. Make sure the toolbox is displayed.
4. Right-click on the toolbox and select **Choose Items...**.
5. Click on the **Browse** button.
6. Navigate to the `/bin` folder within the DNN source (`DNNSource/website/bin`).

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_206_0.png)

7. Select the DotNetNuke.WebControls.dll and click on **Open**.
8. Make sure the DNN controls are checked and click on **OK**.
9. The web controls will now appear under the **General** section of the toolbox when you edit your code.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_207_0.png)

10. Next, we need to add a reference to DotNetNuke.WebUtility.dll. Right-click on the Employee project in the Solution Explorer and select **Add Reference...**.
11. In the pop-up dialog, click on the **Browse** tab and navigate to the folder holding the DNN source files (for example, My Documents\DNNSource\Website\bin).
12. Select the file DotNetNuke.WebUtility.dll and click on **OK**.

### Showing an e-mail link in a DataGrid

As we have seen in the previous chapter, the DataGrid control is perfect for showing records from the database in a neatly formatted table. But the DataGrid can show many other types of information and in this recipe we will see how to display an e-mail hyperlink in a column.

### Getting ready

In this recipe, we will extend the DataGrid from the end of Chapter 6. To follow along you should successfully have completed and deployed the project *Displaying a DataGrid with filter controls*.

> In this recipe we are using a function to generate an e-mail address for our example. This keeps the recipe simple, but isn't really practical. In a real production environment you would store this in the database as part of the Employee table.

### How to do it...

1. Launch the Development Tool and load the **Employee** project.
2. Double-click to open the `ViewEmployee.ascx` file.
3. Locate the DataGrid in the code and add a new column just after the **Salary** column:

```aspx
<dnn:textcolumn datafield="Salary" headertext="Salary"/>

<asp:TemplateColumn HeaderText="Email Contact">
    <ItemTemplate>
        <asp:HyperLink id="hlEmail"
                       NavigateUrl='<%# "mailto:" & DataBinder.Eval(Container.DataItem,"ContactEmail") %>'
                       Text='<%# DataBinder.Eval(Container.DataItem,"ContactEmail") %>'
                       Target="_new"
                       runat="server" />
    </ItemTemplate>
</asp:TemplateColumn>

</Columns>
</asp:datagrid>
```

4. Next, open the `EmployeeInfo.vb` file.
5. Find the Public Properties section and add the read-only property `EmailAddress` to provide an e-mail address constructed from the employee name:

```vb
' public properties

Public ReadOnly Property EmailAddress() As String
    Get
        Return _EmpFirstName.Substring(0, 1) + _EmpLastName + "@yourcompany.com"
    End Get
End Property
```

6. Select **Save All** from the **File** menu.
7. To check the results, build and deploy the module to a development portal for testing (for instructions see the recipe *Deploying a module as a standalone package* in Chapter 5).
8. Go to the ACME Employee page to see the list of employees. The new e-mail hyperlink will appear on the right-hand side.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_209_0.png)

### How it works...

In this recipe we saw the tasks to show an e-mail hyperlink in a DataGrid control:

- We took the DataGrid control from the previous recipe and added a new template column holding an e-mail hyperlink control.
- We added a new property to the EmployeeInfo object to provide an e-mail address for the DataGrid.

### Showing checkboxes in a DataGrid

An element that is useful to display in a DataGrid is a checkbox-like image to indicate the status of the database record. These are not functioning checkboxes but rather a visual indicator showing the data to be true or false.

The control works by having two images, one with a checkmark that is shown when the value is true. The other is an unchecked image that is shown when the value is false.

> This recipe will work with any image indicating true or false. Checkbox-like images are used in other DNN modules so they are familiar to users, but you can experiment with your own images as well.

This recipe has two basic steps:

- We will create a new property of the EmployeeInfo object called `NewHire`. This property checks the date of hire from the database and returns true if the employee was hired less than 30 days ago.
- We will add a new column to the DataGrid that evaluates the `NewHire` property and shows one image if the `NewHire` is true and another image if the `NewHire` is false.

### Getting ready

In this recipe we will extend the DataGrid that we created in *Chapter 6*. To follow along you should have successfully completed and deployed the project *Displaying a DataGrid with filter controls*.

### How to do it...

1. Launch the Development Tool and load the **Employee** project.
2. Double-click to open the `ViewEmployee.ascx` file.
3. The first step is to add a new column to the DataGrid that will show the checkbox images. We will use the `Eval` function to check the `NewHire` property. Locate the DataGrid and add a new column just after the `Salary` column:

```aspx
<dnn:textcolumn datafield="Salary" headertext="Salary"/>

<asp:TemplateColumn HeaderText="New Hire">
    <ItemTemplate>
        <asp:Image Runat="server" ID="imgApproved"
                   ImageUrl="~/images/checked.gif" Visible='<%# DataBinder.Eval(Container.DataItem,"NewHire") = "1" %>'/>
        <asp:Image Runat="server" ID="imgNotApproved"
                   ImageUrl="~/images/unchecked.gif" Visible='<%# DataBinder.Eval(Container.DataItem,"NewHire") = "0" %>'/>
    </ItemTemplate>
</asp:TemplateColumn>

</Columns>
</asp:datagrid>
```

4. Next, open the `EmployeeInfo.vb` file.
5. Find the `Public Properties` section and add the read-only property `NewHire` that returns true or false if the employee was hired in the last 30 days:

```vb
' public properties

Public ReadOnly Property NewHire() As Boolean
    Get
        Return (Today - _HireDate).Days < 30
    End Get
End Property
```
