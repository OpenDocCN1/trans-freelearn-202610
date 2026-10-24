We will use the MBRIntersects criteria here, which provides a performance advantage over a regular Intersects function as it only checks for the intersection of the extent (bounding box). In this example, we are dealing with a few features of limited complexity that are not done dynamically during a web request, so this shortcut does not provide a major advantage—we do this here so as to demonstrate its use for more complicated datasets.

+   1. If it isn't already open, open DB Manager.
+   2. Navigate to Database | SQL window.

![](bbox=[0.207, 0.3, 0.792, 0.77])

Fill the respective input fields in the SQL query dialog: