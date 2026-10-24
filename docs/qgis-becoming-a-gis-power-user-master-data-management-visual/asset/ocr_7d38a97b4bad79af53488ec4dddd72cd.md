+   5. Choose a location where your backup will be saved, adjust the backup options according to your needs, and click on the **Backup** button to start the backup process. The progress will be displayed in the **Messages** tab.

To restore the database from the backup, perform the following steps:

+   1. Start pgAdmin by clicking on its desktop shortcut or by finding it in the **Start** menu.

+   2. Create a connection to your database server if it does not exist by navigating to **File | Add Server...** or clicking on the corresponding button on the toolbar.

+   3. Connect to the database server where you want to restore the backup and double-click on its name in **Object Browser** on the left-hand side of the pgAdmin window.

+   4. Select database that should be restored, open its context menu by clicking the right mouse button, and select **Restore**. A restore options dialog will be opened, as shown in the following screenshot:

![](bbox=[0.27, 0.4, 0.72, 0.67])

> It is worth mentioning that the pg_restore tool used by pgAdmin to restore cannot create the database that has to be restored. It is necessary to create a new empty database manually and then start the restoration with this freshly created database.