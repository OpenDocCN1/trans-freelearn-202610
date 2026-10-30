
## Starting a Revit template

We will start a new Revit template by using the existing `DefaultGBRENU.rte` template that is saved when Revit gets installed. There are multiple options for templates, as we will explain next.

The templates for multiple languages can be found here:

`C:\ProgramData\Autodesk\RVT 2021\Templates`

Metric templates in English can be found here:

`C:\ProgramData\Autodesk\RVT 2021\Templates\English`

Imperial templates in English can be found here:

`C:\ProgramData\Autodesk\RVT 2021\Templates\English-Imperial`

We need to start a **New** project by selecting an existing template (`DefaultGBRENU.rte`) and selecting **Save As** to save the template inside a different folder with a new name. This is so that we can start editing the file:

![](img/1961add582dd4a589b2eac3dda140e38_111_0.png)

Figure 3.3 – Opening a Revit template

After opening the template, we need to save the file. To save the file, navigate to **File | Save As | Template** and rename the file accordingly.

After creating the new template, we need to add it as an option when creating new projects. To add the new template created, please follow the process explained earlier in this chapter.

Our new template will be available to start a new project. Now, we are ready to start configuring the template file as required.

## General settings

The Revit General Settings cover the basic configuration. It is divided into the following items:

- Browser Organization
- Snaps
- Project Information
- Project parameters
- Shared parameters
- Global parameters
- Project units
- Family name conventions
- Exporting **Industry Foundation Classes (IFC)** settings
- IFC mapping for specific project parameters (assigned to objects)
- Keynote settings
- Worksets

## Browser Organization

The Browser Organization tool is essential to increase productivity in Revit. This is because you can optimize the way you organize views and sheets to suit your company's workflow.

## Organizing views by view types

When using view types to organize the Project Browser, it's important to rename the view types accordingly.

To rename views, select a view from the Project Browser and perform the steps in the following screenshot to rename a view type:

![](img/1961add582dd4a589b2eac3dda140e38_114_0.png)

Figure 3.5 – Renaming a view type

During the process of creating views and renaming view types, we recommend that you create a new **Browser Organization method**.

To do that, follow these steps:

1. Hover your mouse over **Views (all)** in the Project Browser. Then, right-click and select **Browser Organization**:

![](img/1961add582dd4a589b2eac3dda140e38_114_1.png)

Figure 3.6 – Browser Organization

2. Click on **New** and type a new name, for example, `FD_View_Type`:

Figure 3.7 – Adding a name for your Browser Organization

3. Click on OK.

When you click on OK, a new window opens that asks you to define the properties you want to use. These properties are grouped under two different tabs:

- **Filtering:** This allows views to be filtered.

  Filter by: Here, select Sheet Name Equal to <None>. This will make the views invisible when they are dropped into a sheet.

Figure 3.8 – Browser Organization Properties – Filtering

- **Grouping and Sorting:** This allows views to be grouped and sorted according to the properties you want.

  **Group by:** Here, select **Type**. This will group the views by **View Type**.

  **Sort by:** Here, select **Associated Level** and then select **Descending**. This will sort the plan views based on the Floor Plan height:

![](img/1961add582dd4a589b2eac3dda140e38_116_0.png)

Figure 3.9 – Browser Organization Properties – Grouping and Sorting

4. Click on **OK**.

5. Make sure that **FD_View_Type** is checked and then click on **OK**.

![](img/1961add582dd4a589b2eac3dda140e38_117_0.png)

Figure 3.10 – Creating a new view Browser Organization

> **Tip**
>
> It is recommended that you delete the existing Browser Organization. Any other combination can be created as required.

In this example, we used View Types, which help you to identify where the views are dropped:

![](img/1961add582dd4a589b2eac3dda140e38_117_1.png)

Figure 3.11 – Viewing the browser according to the view type

## Organizing views by project parameters

In this process, we need to create a new Browser Organization type to use project parameters instead of view types to organize views.

Follow the process we have already demonstrated to create a new Browser Organization type and give it the name **FD_General**:

![](img/1961add582dd4a589b2eac3dda140e38_118_0.png)

Figure 3.12 – Viewing the Browser Organization type based on the project parameters

To organize the Project Browser using a project parameter, we will start creating two parameters for the views.

### View Parameters

- View Package
- View Sub-Code

1. From the Ribbon, under **Manage**, click on **Project Parameters**:

![](img/1961add582dd4a589b2eac3dda140e38_119_0.png)

Figure 3.13 – Creating new project parameters

2. A window will pop up with all of the parameters already created in the template:

![](img/1961add582dd4a589b2eac3dda140e38_119_1.png)

Figure 3.14 – Adding new project parameters

3. Click on **Add…** to open a new window.

This window is divided into three areas:

- **Parameter Type:** This is where users can specify whether the parameter is a project parameter or a shared parameter.
- **Parameter Data:** This is where users can specify what kind of parameter will be created.
- **Categories:** This is where users can assign this new parameter to specific Revit categories.

4. Use these settings to create new parameters, View Package and View Sub-Code:

![](img/1961add582dd4a589b2eac3dda140e38_120_0.png)

Figure 3.15 – New project parameters

5. Now, repeat the process to create the View Sub-Code parameter. For this parameter, it is not necessary to select the Schedules category.

> **Tip**
>
> If the parameter properties need to be changed for some reason, under the Project Parameters window, select the parameter name and click on Modify. Keep in mind that it is not possible to change some of the properties after creating the parameter; in this scenario, the parameter needs to be deleted and created from scratch. If any information is already filled in, it will be deleted from the template (or the project).

6. Once the two new parameters are created, we can configure the FD_General Browser Organization.

In this example, we will only be using the Grouping and Sorting tab:

- **Grouping and Sorting:** This allows views to be grouped and sorted according to the desired properties.

  **Group by:** Here, select View Package. This will group the views according to what will be filled under this parameter.

  **Then by:** Here, select View Sub-Code. This will group the views according to what will be filled under this parameter.

  **Sort by:** Here, select Associated Level and then select Descending. This will sort the plan views based on the Floor Plan height:

![](img/1961add582dd4a589b2eac3dda140e38_121_0.png)

Figure 3.16 – Editing View Organization – FD_General

7. Click on OK.

8. Make sure that FD_General is checked and then click on OK.

In this example, we used Browser Organization to place views into groups divided by new parameters, View Package and View Sub-Code.

We suggest having five View Package options in the template. These and different folders can be created by filling in the two parameters when creating the views. These packages are organized as follows:

- **00 – Administration Views:** These views are related to the template definitions.
- **01 – WIP Views:** This is where all the work-in-progress views should be kept.
- **02 – Views Export:** These are the views that need to be exported into different formats and for different usages.
- **03 – User Views:** These are the views created and kept for a specific reason by a user.
- **04 – Sheet Views:** These are the views that will be dropped into sheets.

The Project Browser will be organized as per the following screenshot:

![](img/1961add582dd4a589b2eac3dda140e38_122_0.png)

Figure 3.17 – Views Organization – FD_General
