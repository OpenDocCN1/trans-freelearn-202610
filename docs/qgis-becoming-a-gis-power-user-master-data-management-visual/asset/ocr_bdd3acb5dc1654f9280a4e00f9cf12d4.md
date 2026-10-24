# Demonstrating Change

+   - The following expression is the difference of each year from the previous year divided by the previous year to find the fractional change. This is then divided by three to find the average over three years and finally multiplied by 100 to find the percentage, as follows:
+   
+   ```text
+   ((("ACS_11_5_2" - "ACS_10_5_2") / "ACS_10_5_2") +
+    (("ACS_12_5_2" - "ACS_11_5_2") / "ACS_11_5_2") +
+    (("ACS_13_5_2" - "ACS_12_5_2") / "ACS_12_5_2")) / 3 * 100
+   ```

+   4. After this, click on **OK**.

# The spatial join in SpatiaLite

Now that we have a value for the average change in white population by tract, let's attach this to the unit of interest, which are the State House Districts. We will do this by doing a spatial join, specifically by joining all the records that intersect our House District bounds to that House District. As more than one tract will intersect each State House District, we'll need to aggregate the attribute data from the intersected tracts to match with the single district that the tracts will be joined to.

We will use SpatiaLite for doing this. Similar to PostGIS for Postgres, SpatiaLite is the spatial extension for SQLite. It is file-based; rather than requiring a continuous server listening for connections, a database is stored on a file, and client programs directly connect to it. Also, SpatiaLite comes with QGIS out of the box, making it very easy to begin to use. As with PostGIS, SpatiaLite comes with a rich set of spatial relationship functions, making it a good choice when the existing plugins do not support the relationship we are trying to model.

> > SpatiaLite is usually not chosen as a database for live websites because of some limitations related to multiuser transactions—which is why CartoDB uses Postgres as its backend database.

# Creating a SpatiaLite database

To do this, perform the following steps:

+   1. Create a new SpatiaLite database.
+   2. Navigate to **Layer | Create Layer | New SpatiaLite Layer**.
+   3. Using the ellipses button (...), browse to and create a database at `c5/data/output/district_join.sqlite`.
+   4. After clicking on **Save**, you will be notified that a new database has been registered. You have now created a new SpatiaLite database. You can now close this dialog.