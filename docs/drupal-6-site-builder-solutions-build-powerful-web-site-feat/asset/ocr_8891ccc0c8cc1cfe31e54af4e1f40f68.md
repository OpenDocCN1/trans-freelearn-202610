When you are ready to restore the web site from backup, you simply reverse the process.

+   1. You should always import into a blank database, to avoid conflicts with existing data. You can either drop or delete all of the titles in the existing database, or you can create a new database to import the data into.
+   2. After you have cleaned out your database, select the Import tab in phpMyAdmin.

![](bbox=[0.109, 0.239, 0.952, 0.622])

+   3. Now navigate to the file that you exported earlier, and click Go to begin the import. You may need to delete all of the tables in the database before you import the data, depending on the options you chose when you exported the data.
+   4. To reload the files, simply open your FTP client, select the same directories that you used when creating the backup and then upload the files, rather than downloading them.