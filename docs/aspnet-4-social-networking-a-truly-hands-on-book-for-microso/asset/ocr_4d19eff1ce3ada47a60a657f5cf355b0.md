As you can see in the previous screenshot we have the owner (**AccountID**) and the folder that is to be owned (**FolderID**).

# Account files

Similar to the AccountFolders table, the AccountFiles table allows someone to subscribe to a specific file. This could be used for the purposes of **Favorites** or similar concepts. The makeup of this table is identical to AccountFolders. You have the owner and the file being owned.

# Folder files

The FolderFiles table allows an Account to not only subscribe to a file, similar to the Favorites concept, but it also allows a user to take one of my files and put it into one of their folders as though the file itself belonged to them.

As you can see in the previous screenshot this is primarily a table that holds the keys to the other tables. We have the **FolderID**, **FileID**, and **AccountID** for each file. This clearly specifies who is taking ownership of what and where they want it to be placed.

# Creating the relationships

Once all the tables are created we can then create all the relationships.

For this set of tables we have relationships between the following tables:

+   - **Files and FileSystemFolders**
+   - **Files and FileTypes**
+   - **Files and Folders**
+   - **Files and Accounts**
+   - **Folders and Accounts**
+   - **Folders and FolderTypes**
+   - AccountFolders and Accounts
+   - AccountFolders and Folders
+   - AccountFiles and Accounts
+   - AccountFiles and Files
+   - FolderFiles and Accounts
+   - FolderFiles and Folders
+   - FolderFiles and Files