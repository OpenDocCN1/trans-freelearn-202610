# 17

# MySQL 5.0 and 5.1 Support

MySQL 5.0 introduced a number of new features. The announcement of these features calmed down a number of developers and industry observers who were claiming that MySQL was an inferior product as compared to some competitors' products, which already had these features. Views, stored procedures, triggers, a standard INFORMATION_SCHEMA, and (more recently) a profiling mechanism are now present in the MySQL spectrum.

Among the new features of MySQL 5.1, the ones that relate to a web interface (for example, partitioning and events) are supported in phpMyAdmin.

# Views

MySQL 5.0 introduced support for the named, and updatable views. A view is a derived table (consider it a virtual table) whose definition is stored in the database. A SELECT statement done on one or more tables (or even on views), is stored as a view and can also be queried.

Views can be used to:

+   - Limit the visibility of columns (for example, do not show the salary)
+   - Limit the visibility of rows (for example, do not show data for specific world regions)
+   - Hide a changed table structure (so that legacy applications can continue to work)

As a view, by itself, has some permissions attached to it, it is easier to GRANT permissions on the view as a whole, rather than define cumbersome, column-specific privileges on the underlying tables.