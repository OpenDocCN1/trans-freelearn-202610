
## Importing SQL files

Any file containing MySQL statements can be imported via this mechanism. The dialog is available in the Database view or the Table view, via the Import subpage, or in the Query window.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_168_0.png)

> There is no relation between the currently selected table (here author) and the actual contents of the SQL file that will be imported. All the contents of the SQL file will be imported, and it is those contents that determine which tables or databases are affected. However, if the imported file does not contain any SQL statements to select a database, all statements in the imported file will be executed on the currently selected database.

Let's try an import exercise. First, we make sure that we have a current SQL export of the book table (as explained in Chapter 7). This export file must contain the structure and the data. Then we drop the book table—yes, really! We could also simply rename it. (See Chapter 10 for the procedure.)

Now it is time to import the file back. We should be on the Import subpage, where we can see the Location of the text file dialog. We just have to hit the Browse button and choose our file.

phpMyAdmin is able to detect which compression method (if any) has been applied to the file. Depending on the phpMyAdmin version, and the extensions that are available in the PHP component of the web server, there is variation in the formats that the program can decompress.

However, to import successfully, phpMyAdmin must be informed of the character set of the file to be imported. The default value is **utf8**. However, if we know that the import file was created with another character set, we should specify it here.

An **SQL compatibility mode** selector is available at import time. This mode should be adjusted to match the actual data that we are about to import, according to the type of the server where the data was previously exported.

To start the import, we click **Go**. The import procedure continues and we receive a message: **Import has been successfully finished, 2 queries executed**. We can browse our newly-created tables to confirm the success of the import operation.

The file could be imported for testing in a different database or even in a MySQL server.

## Importing CSV files

In this section, we will examine how to import CSV files. There are two possible methods – **CSV** and **CSV using LOAD DATA**. The first method is implemented internally by phpMyAdmin and is the recommended one for its simplicity. With the second method, phpMyAdmin receives the file to be loaded, and passes it to MySQL. In theory, this method should be faster. However, it has more requirements due to MySQL itself (see the *Requirements* sub-section of the *CSV using LOAD DATA* section).

## Differences between SQL and CSV formats

There are some differences between these two formats. The CSV file format contains data only, so we must already have an existing table in place. This table does not need to have the same structure as the original table (from which the data comes); the **Column names** dialog enables us to choose which columns are affected in the target table.

Because the table must exist prior to the import, the CSV import dialog is available only from the **Import** subpage in the Table view, and not in the Database view.

## Exporting a test file

Before trying an import, let's generate an author.csv export file from the author table. We use the default values in the CSV export options. We can then Empty the author table—we should avoid dropping this table because we still need the table structure.

## CSV

From the author table menu, we select Import and then CSV.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_170_0.png)

We can influence the behavior of the import in a number of ways. By default, importing does not modify existing data (based on primary or unique keys). However, the Replace table data with file option instructs phpMyAdmin to use REPLACE statement instead of INSERT statement, so that existing rows are replaced with the imported data.

Using Ignore duplicate rows, INSERT IGNORE statements are generated. These cause MySQL to ignore any duplicate key problems during insertion. A duplicate key from the import file does not replace existing data, and the procedure continues for the next line of CSV data.

We can then specify the character that terminates each field, the character that encloses data, and the character that escapes the enclosing character. Usually this is \. For example, for a double quote enclosing character, if the data field contains a double quote, it must be expressed as "some data \" some other data".

For Lines terminated by, recent versions of phpMyAdmin offer the auto choice, which should be tried first as it detects the end-of-line character automatically. We can also specify manually which characters terminate the lines. The usual choice is \n for UNIX-based systems, \r\n for DOS or Windows systems, and \r for Mac-based system (up to Mac OS 9). If in doubt, we can use a hexadecimal file editor on our client computer (not part of phpMyAdmin) to examine the exact codes.

By default, phpMyAdmin expects a CSV file with the same number of fields and the same field order as the target table. But this can be changed by entering a comma-separated list of column names in Column names, respecting the source file format. For example, let's say our source file contains only the author ID and the author name information:

```
"1","John Smith"
"2","Maria Sunshine"
```

We'd have to put id, name in Column names to match the source file.

When we click Go, the import is executed and we get a confirmation. We might also see the actual INSERT queries generated if the total size of the file is not too big.

```
Import has been successfully finished, 2 queries executed.
INSERT INTO `author` VALUES ('1', 'John Smith', '+01 445 789-1234')
# 1 row(s) affected.

INSERT INTO `author` VALUES ('2', 'Maria Sunshine', '333-3333')
# 1 row(s) affected.
```

## CSV using LOAD DATA

With this method, phpMyAdmin relies on the server's LOAD DATA INFILE or LOAD DATA LOCAL INFILE mechanisms to do the actual import, instead of processing the data internally. These statements are the fastest way for importing text in MySQL. They cause MySQL to start a read operation either from a file located on the MySQL server (LOAD DATA INFILE) or from another place (LOAD DATA LOCAL INFILE), which in this context, is always the web server's file system. If the MySQL server is located on a computer other than the web server, we won't be able to use the LOAD DATA INFILE mechanism.

## Requirements

Relying on the MySQL server has some consequences. Using LOAD DATA INFILE requires that the logged-in user possess a global FILE privilege. Also, the file itself must be readable by the MySQL server's process.

> Chapter 18 explains phpMyAdmin's interface, for system administrators to manage privileges.

Usage of the LOCAL modifier in LOAD DATA LOCAL INFILE must be allowed by the MySQL server and MySQL's client library used by PHP.

Both the LOAD methods are available from the phpMyAdmin LOAD interface, which tries to choose the best possible default option.

## Using the LOAD DATA interface

We select Import from the author table menu. Choosing CSV using LOAD DATA brings up the following dialog:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_172_0.png)

> The available options have already been covered in the CSV section.

In the familiar Location of the text file question, we choose our author.csv file.

Finally, we can choose the LOAD method, as discussed earlier, by selecting the Use LOCAL keyword option. We then click Go.

If all goes well, we can see the confirmation screen as follows:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_172_1.png)

This screen shows the exact **LOAD DATA LOCAL INFILE** statement used. Here is what has happened:

- We chose **author.csv**.
- The contents of this file were transferred over HTTP and received by the web server.
- The PHP component inside the web server saved this file in a work directory (here **/opt/php-upload-tmp/**) and gave it a temporary name.
- phpMyAdmin, informed of the location of this working file, built a **LOAD DATA LOCAL INFILE** command, and sent it to MySQL. Note that just one query was executed, which loaded many rows.
- The MySQL server read and loaded the contents of the file into our target table. It then returned the number of affected rows (2), which phpMyAdmin displayed in the results page.

## Web server upload directories

To get around cases where uploads are completely disabled by a web server's PHP configuration, or where upload limits are too small, phpMyAdmin can read upload files from a special directory located on the web server's file system. This mechanism is applicable for SQL and CSV imports.

We first specify the directory name of our choice in the $cfg['UploadDir'] parameter, for example, './upload'. We can also use the %u string, as described in Chapter 7, to represent the user's name.

Now, let's go back to the Import subpage. We get an error message:

**The directory you set for upload work cannot be reached.**

This error message is expected, as the directory does not exist. It is supposed to have been created inside the current phpMyAdmin installation directory. The message might also indicate that the directory exists, but can't be read by the web server. (In PHP safe mode, the owner of the directory and the owner of the phpMyAdmin-installed scripts must be the same.)

Using an SFTP or FTP client, we create the necessary directory, and can now upload a file there (for example book.sql) bypassing any PHP timeouts or upload maximum limits. Note that the file itself must have permissions that allow the web server to read it. In most cases, the easiest way is to allow everyone to read the file.
