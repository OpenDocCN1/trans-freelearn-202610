# Forms

The forms are in the presentation tier in any three-tier architecture system. Most of them are related to look and feel or layout, which we will discuss it in Chapter 4, *Best Practices – GUI*. Some other best practices for forms revolve around the following characteristics:

+   - Use of Intellimorph maximally
+   - No forced date or time format
+   - No forced layout such as fixed width for label, position control for GUI controls, etc.
+   - Use of label files for GUI text
+   - Forms having minimal coding

# Avoid Coding on Forms

The basic concept of three-tier architecture is that forms should be used only for the presentation tier and hence no other code such as business logic should be there on forms. The code placed on forms also reduces their reusability and the ease of further customization; e.g. if you want to develop an enterprise portal, the code written on forms will have to be written again in classes or table methods, etc., which will make the implementation complex. Another example may be when you want to 'COM enable' your business logic; form code related to business logic will make your life almost impossible.

Any code (other than presentation logic) written on forms imposes limitation on performance as call between two different layers increase slowing the performance and hence code on forms should be avoided as much as possible. In cases where avoiding code on forms is not possible the guidelines summarized in the following table should be used for writing code on forms.

| Place to Write Code | Guidelines |
|---|---|
| Form level | When code is related to whole form |
|  | When code is related to multiple data sources |
|  | Editor or Display methods (only those that are not related to any data source) |
| Data source | Data source-related Edit or Display methods |
|  | Code related only to the data source that cannot be effectively placed in a table method |
| Controls | When it is strictly related to the controls |