
## Configuring relations

In the *Joining layer data recipe*, we discussed that joins only append additional columns to existing features (1:1 or n:1 relationships). Using joins, it is, therefore, not possible to model 1:n relationships, such as "one zip code area containing n schools". These kinds of relationships can instead be modeled using relations. This recipe introduces the concept of relations and shows how you can put them to use.

### Getting ready

To follow this exercise, load zip code areas and schools from `zipcodes_wake.shp` and `schools_wake.shp`.

### How to do it...

Relations are configured in **Project Properties**. The dialog is very similar to the join dialog:

1. Define the two layers (**Referencing/Child** and **Referenced/Parent**), as well as the fields containing the common values/IDs. As you want to model "one zip code area containing n schools," the zip code dataset is the parent layer and the school dataset is the child layer. The connection between both datasets is established based on the zip code fields (**ADDRZIPCOD** and **ZIPNUM**), as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_481_0.png)

2. To verify that the relation is set up and working, you can either check the attribute table in form view (button in the lower-right corner), as shown in the following screenshot, or open an individual feature form. You will find that the relation information has been appended at the end of the form:

![](img/2e88bf5938bf48a409c0a534a8775b21_482_0.png)

As the preceding screenshot shows, setting up this relation enables you to get access to all schools within a certain zip code in a very convenient way. As the edit button suggests, it is even possible to edit the school data from this view. You can simply edit the values in the table view. You can add and delete schools from the dataset using the + and X buttons. The next two buttons enable you to quickly add new entries to the relation or to remove them.

### How it works...

In this example, removing a school from the dataset works just fine, but adding a school via this dialog makes less sense because you cannot create a point geometry through this process.

If you press the button to add to the relation, you will get a dialog that allows you to choose which existing school you want to add. In the background, the school's ADDRZIPCOD value is updated to match the zip code we just assigned it to.

Similarly, if you select a school and press the button to remove the relation, what actually happens is that the school's ADDRZIPCOD value is set to NULL.

## Joining tables in databases

If you use a database (SpatiaLite or PostGIS) to store your data, vector and nonspatial, then you also have the option of using the database and SQL to perform tables joins. The primary advantages of this method include being able to filter data before loading it in the map, perform multitable joins (three or more), and have full control over the details of the join via queries.

### Getting ready

You'll need at least two layers in either a SpatiaLite or PostGIS database. These two layers need at least one column in common, and the column in common should contain unique values in at least one table. In this case, our example uses the `census_wake_2000` polygon layer and `census_wake_2000_pop.csv`.

### How to do it...

1. Open the **DB Manager** plugin that comes with QGIS. You can find this in the **Database** menu.
2. Select your database from the tree on the left-hand side, use `cookbook.db` in **SpatiaLite** (which was created in *Chapter 1, Data Input and Output*).
3. Now, open the SQL window (the second icon from the left in top toolbar of the plugin window).
4. Put in the following SQL code to query and JOIN the tables:

![](img/2e88bf5938bf48a409c0a534a8775b21_483_0.png)

```sql
SELECT *
FROM census_wake2000 AS a
JOIN census_wake2000_pop AS b
ON a.stfid = b.stfid;
```

### How it works...

SELECT lists all the columns that you want from the source tables; in this case, * means everything. FROM is the first (left) table, and a is an alias, which is used so that there's less typing later. JOIN is the second (right) table, and ON indicates which columns should be matched between the two tables. The rest of how this works in relational database theory is best explained in other texts.

### There's more...

In databases, there's more than one type of join. You can perform a join where you retain only the matches in both tables, or you can retain all content from the left (first table) and any matches from the right. You can also control how a one-to-many relationship is summarized or select specific records instead of aggregating.

If you want to save the results of a query you have two options. You can make a view or a new table. A view is a saved copy of your query. Every time you open it, the query will be rerun. This is great if your data changes because it will always be up-to-date, and this doesn't use any additional disk space. On the other hand, a table is like saving a new file; it becomes a static new copy of the results. This is good to repeatedly access the same answer, and it is usually faster to use, especially for large tables.

### See also

- Refer to the *Creating views in SpatiaLite* and *Creating views in PostGIS* sections in this chapter to learn how to make views of the query results.
- For more general information on writing SQL queries refer to http://sqlzoo.net/
- Refer to *Chapter 1, Data Input and Output,* about using the `cookbook.db` database

## Creating views in SpatiaLite

In a database, view is a stored query. Every time you open it, the query is run and fresh results are generated. To use views as layers in QGIS takes a couple of steps.

### Getting ready

For this recipe, you'll need a query that returns results containing a geometry. The example that we'll use is the query from the *Joining tables in databases* recipe (the previous recipe) where attributes were joined 1:1 between the census polygons and the population CSV. The QSpatiaLite plugin is recommended for this recipe.

### How to do it...

The GUI method is described as follows:

1. Using the **QspatialLite** plugin (which is in the **Database** menu, if you've activated it) place the following in the query:

```sql
SELECT *
FROM census_wake2000 as a
JOIN census_wake2000_pop as b
ON a.stfid = b.stfid;
```

2. From the Option dropdown, select the last choice, Create Spatial View & Load in QGIS, and set the Geometry field box value to the name of your geometry field from your spatial layer. In this example, this is geom.

![](img/2e88bf5938bf48a409c0a534a8775b21_485_0.png)

> You can explore your data table fields in the left-hand side to check the name of the fields that you need.

The SQL method is as described, as follows:

1. In Database | DB Manager, open SQL Window.
2. Write a query. In this example, this is the Join query from the previous recipe.
3. Convert this query to a view by adding CREATE VIEW <name> as SELECT:

```sql
CREATE VIEW census_wake2000_pop_join AS
SELECT *
FROM census_wake2000 as a
JOIN census_wake2000_pop as b
ON a.stfid = b.stfid;
```

4. Register the view with the SpatiaLite metadata backend with a follow up query. This function is case sensitive:

```sql
INSERT INTO views_geometry_columns
(view_name, view_geometry, view_rowid, f_table_name,
f_geometry_column,read_only)
VALUES ('census_wake2000_pop_join', 'geom', 'rowid',
'census_wake2000', 'geom',1);
```

5. The pattern is ('name of view','name of view geometry field','A Unique ID','name of table the view gets its geometry from','name of geometry field in the original table','read-only (1) or writable(0)).

6. After running the second query, you should be able to load the view in QGIS and see the same fields as the join query.

### How it works...

A view is actually stored in the database and is triggered when you load it. In this way, if you change the original data tables, the view will always be up to date. By comparison, creating new tables makes copies of the existing data, which is stored in a new place, or creates a snapshot or freeze of the values at that time. It also increases the database's size by replicating data. Whereas, a view is just the SQL text itself and doesn't store any additional data.

QGIS reads the metadata tables of SpatiaLite in order to figure out what layers contain spatial data, what kind of spatial data they contain, and which column contains the geometry definition. Without creating entries in the metadata, the tables appear as normal SQLite tables, and you can only load attribute data without spatial representation.

As it's a view, it's really reading the geometries from the original tables. Therefore, any edits to the original table will show up. New in SpatiaLite 4.x series, this makes it easier to create writable views. If you use the spatialite-gui standalone application, it registers all the database triggers needed to make it work, and the changes made will affect the original tables.
