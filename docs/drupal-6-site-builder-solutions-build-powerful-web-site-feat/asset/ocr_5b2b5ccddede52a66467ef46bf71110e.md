We will discuss ways to optimize both the back end database, and the front end of the web site that displays the information to the visitor.

# Optimizing the database tables

*Good Eatin' Goal:* Optimize the database where your site content is stored, to ensure the best possible performance.

*Additional modules needed:* DB Maintenance (`http://drupal.org/project/db_maintenance`).

# Basic steps

When a database is used as frequently as the Drupal database is, it can become less optimized over time, because having a large number of records inserted and deleted from each table can result in the database having wasted space. All databases will allow you to run an optimization process that removes unused space and rebuilds the indexes to make the search for information faster.

The **DB Maintenance** module can automatically perform the optimization process for you when cron is run.

+   1. Begin by downloading and installing the **DB Maintenance** module.
+   2. You can edit the settings for the **DB Maintenance** module by selecting **Site configuration** and then **DB maintenance**, from the **Administer** menu.
+   3. The settings allow you to control several options, including:
+      a) Whether or not to write to the log when each optimization is performed.

![](bbox=[0.07, 0.621, 0.91, 0.804])