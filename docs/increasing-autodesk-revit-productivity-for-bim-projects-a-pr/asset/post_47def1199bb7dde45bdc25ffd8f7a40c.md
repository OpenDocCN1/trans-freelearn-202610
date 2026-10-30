
You can find and download the latest updates from the following locations:

- The Autodesk Manage Account portal:

![](img/1961add582dd4a589b2eac3dda140e38_34_0.png)

Figure 1.13 – The Autodesk Manage Account portal

- The Autodesk Desktop App:

![](img/1961add582dd4a589b2eac3dda140e38_34_1.png)

Figure 1.14 – Autodesk Desktop App

> Tip
> It is not recommended that you update the Revit release immediately after the new version is rolled out.

## Revit's recommended settings

Revit is a high-demanding software in terms of using up computer resources. It is highly recommended that you keep the settings as least demanding as possible. We will only highlight a few recommendations, but others might also apply, depending on which discipline it is or even the task you want to do.

The following is accessible through the Revit **Options button**:

![](img/1961add582dd4a589b2eac3dda140e38_35_0.png)

> Tip
> This *hardware acceleration* feature should be disabled when navigating through a view, when users are experiencing graphics-related issues, or when this feature is incompatible with the graphics card.

## File size

A Revit file size doesn't depend only on the project size but on many other factors too, such as the geometry complexity, file formats imported, amount of families imported, and any warnings.

Auditing the models every week will help to keep the model both in a workable condition and healthy without affecting or reducing the Revit performance.

As a reference, and depending on your computer's specifications, Revit models should not be bigger than 300 MB.

If the file needs to be bigger, users must decide whether it should be kept as a single model/volume, or if the model segregation and model volume breakdown should be reviewed.

For a federated model (that is, files with multiple links), the recommended maximum file size is approximately 500 MB.

> Tip
> As a good practice, both project and family files should be purged to reduce the file size by deleting any unwanted or unused elements and settings.

## Family creation

Family creation is one of the big topics when it comes to performance, as the more complex we develop them (for instance, with multiple formulas, over-modeling, nested families, arrays, voids, or overconstrained with parameters), the bigger the file becomes.

When creating families, users should bear the following in mind:

- Standard families should be used instead of model in-place families (especially when they will be repeated).
- Nested families must be limited to a certain level – they should be kept to one nest level, if possible.
- **The excluding elements option:** Users should use the Exclude Members feature for minor exclusion.
- **Design options:** Users should avoid using groups in design options.
- **Levels constraints:** Users should not constrain elements to levels (for example, a top constraint of a wall).
- **System families:** Users should avoid using system families in groups as they are constrained to a specific level. When copied to different levels, the first level reference will be kept, and any further editing will get more complicated (remember that the further we are from the reference level, the lower the performance we will get).
- **Worksets:** On a worksharing project, when creating a group with elements from different worksets, Revit will ignore the existing worksets and will include all elements in the current workset when creating the group – this workset cannot be edited.
- **Links:** Users should not add Revit links to a group as it will be duplicated and, consequently, affect performance.

## In-place families

In-place families are bespoke families that users can create within a project context. They should not be used in a project because they will reduce performance and a standard family (that is, a loadable family) can cover most situations:

![](img/1961add582dd4a589b2eac3dda140e38_39_0.png)

Figure 1.18 – Model In-Place families

If users create an in-place family to be used more than once, they should use the standard family (the loadable family). This is because if an in-place family is copied across, Revit will deal with these instances as different families. This will duplicate the family multiple times, which will affect the performance:

![](img/1961add582dd4a589b2eac3dda140e38_40_0.png)

Figure 1.19 – Duplicated In-Place families

## Arrays

When using arrays to copy elements, users should decide whether elements should be kept grouped or not grouped, as this will affect the performance as well. Groups should be kept as minimally associated as possible. Otherwise, they should be ungrouped:

![](img/1961add582dd4a589b2eac3dda140e38_40_1.png)

Figure 1.20 – Creating groups within the Array tool

## Constraints

Using **Constraints** will affect model the performance. They should be used sparingly across the project.

Constraints can include the following:

- Join geometry constraints
- Dimensional constraints
- Lock alignment constraints

When using these constraints, bear in mind that it is almost impossible to see where they are as they can be placed in any view.

Revit has, underneath the **View Control** bar, the **Reveal Constraints** tool, which allows you to check dimensional and locked alignment constraints by adding a kind of "filter" on top of the view. When using the tool, Revit will apply a garnet border to wrap the view, and all constraints will be highlighted in the same color:

![](img/1961add582dd4a589b2eac3dda140e38_41_0.png)

## When using a worksharing environment

Worksharing is a working method in Revit that allows team members to work on the same model at the same time:

![](img/1961add582dd4a589b2eac3dda140e38_42_0.png)

Figure 1.22 – A worksharing environment

In this working methodology, Revit needs to work with worksets to accommodate such environments.

There are a few native tools in Revit related to the worksharing method that require an in-depth understanding, and we will be covering them in the upcoming chapters of this book.

Worksets are crucial during this working method, as users can keep ownership of the elements and/or views. Typically, users will be assigned to specific areas of the project or even to a specific functional area to work on.

Based on this process of managing elements or parts of the project, Revit allows users to open/close worksets not only when opening the file, but also when working in views. Therefore, Revit will become faster as if we have worksets switched off, it doesn't need to process all information when working in the model.

Recommendations can be followed, but we believe that communication is the way to succeed. It is important to keep all team members informed about what is happening in the design process and how it might affect model organization and even the workset structure.

> Tip
> There are several tools that you can use to enhance team collaboration and communication, such as Skype, Microsoft Teams, Microsoft OneNote, Microsoft Planner, Slack, and more.

## Opening a worksharing model

When opening a worksharing model, Revit allows users to select which worksets should be open. If you close a workset that is not required, Revit will open the file faster and, consequently, improve performance. The closed worksets will not be visible, so you can keep only the information that you need open.

To select which worksets users can open or close, follow these steps:

1. Go to the File menu, select Open, and then click Project.
2. Select the Revit Central model to open.
3. On the right-hand side of the **Open** button, click on the drop-down arrow and select **Specify…**:

![](img/1961add582dd4a589b2eac3dda140e38_44_0.png)

Figure 1.23 – Specifying which worksets to open

4. An **Opening Worksets** window pops up. By selecting particular worksets, it's possible to **Open/Close** them as needed by clicking on the **Open** and/or **Close** buttons:
