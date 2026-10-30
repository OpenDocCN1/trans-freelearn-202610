
- **Model Objects:** With this, you can configure all model objects from all disciplines in the project.
- **Annotation Objects:** With this, you can configure all annotation elements in the project.
- **Analytical Objects:** With this, you can configure all analytical elements (including the structural disciplines and zones) of the project.
- **Imported Objects:** With this, you can configure all of the imported elements in the project (that is, those imported directly into the project or a family).

## Fill patterns

Fill patterns can be used in Revit to define the appearance of the surfaces of objects. Regarding 3D elements, it can define the appearance not only from surfaces but also from the cut areas. Follow these steps to create a fill pattern:

- 1. From the ribbon, click on **Manage** and then click on **Fill Patterns**:

![](img/1961add582dd4a589b2eac3dda140e38_167_0.png)

Figure 3.66 – Fill Patterns

Patterns can be created by importing the pattern file from AutoCAD (the PAT file) or created directly in Revit if they are basic patterns.

## Material library

Materials in Revit are not just about rendering; they also control how elements will be displayed in views and add specific data regarding their properties.

From the material, we can combine five different assets:

- **Identity**: This can be used to add technical information about the material.
- **Graphical**: This can be used to control the color and the corresponding patterns to be viewed in a project view.
- **Appearance**: This can be used for rendered views, realistic views, or ray-traced views.
- **Physical**: This can be used for structural analysis.
- **Thermal**: This can be used for energy analysis.

This information can be accessed by clicking on **Manage**, under **Materials**:

![](img/1961add582dd4a589b2eac3dda140e38_169_0.png)

Figure 3.68 – The Materials library

A new window will open where we can add and edit all of the information we need:

![](img/1961add582dd4a589b2eac3dda140e38_170_0.png)

Figure 3.69 – Material Browser

This window can be expanded or collapsed when you click on the double arrows:

![](img/1961add582dd4a589b2eac3dda140e38_171_0.png)

Figure 3.70 – Material Browser

As a good practice, we recommend that you do the following:

- Use a proper naming convention to identify the material.
- Under the **Identify** asset, use a proper class for easier filtering.
- When possible, rename the **Physical** and **Thermal** assets as per the material name.
- Create a material library with proper categories for each project.

Tip
Revit Materials can be shared with Autodesk Inventor and Autodesk Civil 3D.

## View templates

You can define view templates as a collection of view properties that can be controlled and assigned to specific views, ensuring the assigned views will use the same view settings and keeping consistency across construction documentation sets.

Before creating view templates, we suggest that you think about how the views will be used in your project. For example, they can be organized per package:

- Site plans
- Floor plans
- Reflective ceiling plans
- Elevations

View templates can be accessed as follows:

- 1. From the Ribbon, click on **View**. Then, click on **View Templates** and **Manage View Templates**:

![](img/1961add582dd4a589b2eac3dda140e38_172_0.png)

Figure 3.71 – Manage View Templates

You can duplicate existing view templates through the new window and rename or delete existing ones.

> **Tip**
> 
> Another workflow to create a view template is to configure the view as required. Then, in the Project Browser, hover the mouse over **View Name**, right-click, and click on **Create View Template from View…**

For better control and organization, we suggest that you define a minimum number of view templates in the template and create more as per your project requirements and needs.

> Tip
> Having as many settings as possible controlled by view templates will keep consistency across all drawings, and it is also an easier way to change any settings.

2. The following screenshot shows a list of all view templates available in the template:

![](img/1961add582dd4a589b2eac3dda140e38_173_0.png)

Figure 3.72 – The View Templates setting

- 3. After creating a view template, you can assign it to a view or a group of views. You can do this by selecting and changing the view properties, under the **View Template** parameter:

![](img/1961add582dd4a589b2eac3dda140e38_174_0.png)

Figure 3.73 – Applying a view template to a view

> **Tip**
> 
> When a view template is assigned to a view, it's not possible to change the view properties that are controlled by the view template. Under the **Visibility** bar, you can use the **Temporary View Properties** option to change the settings temporarily and redefine it as it was by resetting to the view template assigned.

## Filters

Filters can be used not only to override graphical representations, but also the visibility of elements.

View filters can be accessed by performing the following steps:

- 1. From the Ribbon, click on View and then click on Filters:

![](img/1961add582dd4a589b2eac3dda140e38_175_0.png)

Figure 3.74 – View Filters

- 2. From the new window, you can create two different view filters, as follows:

- Rule-based Filters: These are filters that are based on the element categories and parameters.
- Selection Filters: These are filters that are based on selections:

![](img/1961add582dd4a589b2eac3dda140e38_175_1.png)

Figure 3.75 – View Filters settings

These view filters can be added to view templates for better control of the visibility and graphics of your project.

## Color fill legends and schemes

Color fill legends are different graphical ways of applying colors to rooms, areas, and space based on the model data available. They can be used and placed anywhere in the floor plan along with a legend with the color code that is used to fill the model elements.

Color fill legends can be added by navigating to Annotate, under the Color Fill panel:

![](img/1961add582dd4a589b2eac3dda140e38_176_0.png)

Figure 3.76 – Adding color fill schemes

Revit will place the color fill legend and activate the color scheme itself. The legend can be deleted at any time, and the color scheme can be removed or added under Properties:

![](img/1961add582dd4a589b2eac3dda140e38_176_1.png)

Figure 3.77 – Toggling color scheme visibility

Color-coding can be added based on the different data that has been added to the rooms' properties.

## Phases

The Phases feature allows you to work with refurbishment projects. This means that the project will be represented on different project stages: before and after refurbishment. With this tool, Revit will track the phase elements that are **New, Demolished, Existing, and Temporary.**

This feature can only be applied to model elements. By using phase filtering, we can apply color-coding to objects in these four states:

- **Existing:** Elements that exist in the view phase.
- **New:** Elements modeled in the view phase.
- **Demolished:** Existing elements demolished in the view phase (that is, when Phase Created and Phase Demolished are different).
- **Temporary:** New elements are demolished in the same phase (that is, when Phase Created and Phase Demolished are the same).

To access the Phases feature, follow these steps:

- 1. From the ribbon, click on **Manage** and then click on **Phases:**

![](img/1961add582dd4a589b2eac3dda140e38_177_0.png)

Figure 3.78 – The Phases setting

To access the export process, follow these steps:

- 1. From the upper-left corner, click on File. Then click on Export, CAD Formats, and DWG:

![](img/1961add582dd4a589b2eac3dda140e38_181_0.png)

Figure 3.83 – DWG exports
