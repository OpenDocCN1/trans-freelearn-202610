![](bbox=[0.178, 0.148, 0.822, 0.419])

# # Files

The most important thing to consider storing while in the database is of course our primary interest files. As with most other conversations regarding a physical binary file we always have to consider if we want to store the file in the database or on the file system. In this case we think it makes sense to store the file (and in the case of a photo, its various generated sizes) on the file system. This means that we will only be storing metadata about each file in our database.

The most important field here to discuss is the **FileSystemName**. As you can see this is a GUID value. We will be renaming uploaded files to GUIDs in addition to the original extension. This allows us to ensure that all the files in any given folder are uniquely named. This removes the need for us to have to worry about overwriting other files.

Then we see the **FileSystemFolderID**. This is a reference to the **FileSystemFolders** table, that lets us know the root folder location where the file is stored.

Next on our list of items to discuss is the **IsPublicResource** flag. By its name it is quite clear that this flag will set a file as public or private and can therefore be seen by all or by its owner (**AccountID**).