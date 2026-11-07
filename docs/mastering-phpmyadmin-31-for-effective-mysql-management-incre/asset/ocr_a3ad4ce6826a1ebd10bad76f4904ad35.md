# Using the Designer for PDF layout

The Designer feature (available in the Database view) offers a more refined way of moving the tables on screen as the column links follow the table movements. Therefore, an interface exists between the tables' coordinates, as saved by the Designer, and the coordinates for the PDF schema. Let's enter the Designer and click the small PDF logo.

This brings us to a panel where we can choose the (existing) PDF schema name and the action we want to perform — in our case, to export the Designer coordinates to the PDF schema. We could also use the Create a new page dialog, entering a PDF page name and clicking Go to create an empty page. From here, we can subsequently export the coordinates saved from the Designer workspace.

![](bbox=[0.34, 0.35, 0.66, 0.51])

> There is a difference in the span of tables managed by the Designer and by the Edit PDF feature. The Designer manipulates every table of a database, whereas the Edit PDF panel offers us a choice of tables enabling us to represent a subset of the relations in case there are many tables.

# Summary

This chapter covers the documentation features offered by phpMyAdmin — the print view for a database or a table and the data dictionary for a complete column list. The chapter also covers PDF relational schemas. In particular, it focuses on how to create and modify a PDF schema page, and how to use the visual editor (scratchboard). The next chapter will explain how to apply transformations to data, in order to customize its format at view time.