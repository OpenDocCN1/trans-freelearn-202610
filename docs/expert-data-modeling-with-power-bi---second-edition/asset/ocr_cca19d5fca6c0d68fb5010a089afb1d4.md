The following steps show how to create a subfolder nested in the root folder:

+   1. Select the desired measure(s).
+   2. Use a backslash (\\) character to create a subfolder, then press Enter on the keyboard.

The following image shows the preceding steps:

![](bbox=[0.178, 0.283, 0.823, 0.552])

Figure 9.39: Creating subfolders

# Reducing model size by disabling auto date/time

When the data is loaded into the data model, Power BI automatically creates some Date tables to support calendar hierarchies for all columns in the DateTime datatype. This feature is convenient, especially for beginners who do not know how to create a Date table or create and manage hierarchies. However, it can consume too much storage, potentially leading to severe performance issues. As mentioned earlier, the auto date/time feature forces Power BI Desktop to create Date tables for every single