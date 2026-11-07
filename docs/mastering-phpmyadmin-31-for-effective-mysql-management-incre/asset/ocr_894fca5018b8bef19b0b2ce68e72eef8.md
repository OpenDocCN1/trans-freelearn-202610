Notice the following:

+   - The main title of the right panel has changed to reflect the fact that we are now located in this database.
+   - A confirmation message regarding the creation is displayed.
+   - The left panel has been updated; we see **marc_book (0)**. Here, the name indicates that the **marc_book** database has been created, and the number **0** indicates that it contains no tables.
+   - By default, the SQL query sent to the server by phpMyAdmin to create the database is displayed in color.

> phpMyAdmin displays the query it generated, because $cfg['ShowSQL'] is set to TRUE. Looking at the generated queries can be a good way of learning SQL.

As the generated queries could be large and take much on screen room, the $cfg['MaxCharactersInDisplayedSQL'] acts as a limit. Its default value of 1000 should be a good balance between seeing too few and seeing too much of the queries, especially when doing large imports.

It is important to examine the phpMyAdmin feedback to ascertain the validity of the operations we make through the interface. This way, we can detect errors like typos in the names, or creation of a table in the wrong database.

# Creating our first table

Now that we have a new database, it's time to create a table in it. The example table we will use is a table named **book**.

# Choosing the fields

Before creating a table, we should plan the information we want to store. This is usually done during database design. In our case, a simple analysis leads us to the following book related data we want to keep:

+   - **International Standard Book Number (ISBN)**
+   - Title
+   - Number of pages
+   - Author identification