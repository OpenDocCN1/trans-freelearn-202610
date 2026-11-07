# Importing SQL files

Any file containing MySQL statements can be imported via this mechanism. The dialog is available in the Database view or the Table view, via the Import subpage, or in the Query window.

![](bbox=[0.18, 0.23, 0.82, 0.51])

> There is no relation between the currently selected table (here author) and the actual contents of the SQL file that will be imported. All the contents of the SQL file will be imported, and it is those contents that determine which tables or databases are affected. However, if the imported file does not contain any SQL statements to select a database, all statements in the imported file will be executed on the currently selected database.

Let's try an import exercise. First, we make sure that we have a current SQL export of the book table (as explained in Chapter 7). This export file must contain the structure and the data. Then we drop the book table—yes, really! We could also simply rename it. (See Chapter 10 for the procedure.)

Now it is time to import the file back. We should be on the Import subpage, where we can see the Location of the text file dialog. We just have to hit the Browse button and choose our file.