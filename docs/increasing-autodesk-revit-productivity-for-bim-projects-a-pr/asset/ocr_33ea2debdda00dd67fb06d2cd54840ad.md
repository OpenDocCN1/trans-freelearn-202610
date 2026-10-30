The following screenshot shows a family with unnecessary details for most of the design stages:

![](bbox=[0.117, 0.151, 0.849, 0.369])

Figure 10.32 – Skylight over-modeled

Frequently, they are over-modeled, and the family needs to be simplified as a single file can have 5 MB, 10 MB, or even 50 MB (depending on formulas, CAD imports, detailing, and nested families).

As a reminder from *Chapter 1, Introduction to Productivity in Revit*, Revit families should follow these file sizes:

+   - A Revit family file from 250 KB to 500 KB indicates a suitable file size.
+   - A Revit family file over 1 MB (1,000 KB) should be reviewed to reduce the size if possible.
+   - A Revit family file over 2 MB (2,000 KB) should definitely be reviewed.
+   - A nested family file size tends to be bigger, so we need to check the large family files. It is not recommended to have more than two levels of nested families.

# CAD formats imported into families

All imports should be reduced to the minimum as they will directly affect the performance. Some of these geometries can be created with meshes or fabrication software such as **Inventor** or **SolidWorks** (these models are generally over-detailed as they have all the information for fabrication).