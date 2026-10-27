# # Time for action – creating a backup with phpMyAdmin

The steps described here are only needed if you have no possibility to backup the database with the module described previously.

+   1. Log in to phpMyAdmin on your server (contact your provider to figure out the details as they can differ between web hosts).
+   
+   2. Upon login, select **Databases** on the major screen:
+   
+   3. Select the database used for a CMS Made Simple installation. If you are not sure, then open the `config.php` file and find the line `$config['db_name']` that contains the name of the database.
+   
+   4. Click on the database name in the list.
+   
+   5. On the next screen, you will see the list of all tables in this database. Click on the **Export** tab at on the top of the list, as shown in the following screenshot:

![](bbox=[0.357, 0.319, 0.647, 0.538])

![](bbox=[0.182, 0.698, 0.819, 0.851])