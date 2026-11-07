This screen shows the exact **LOAD DATA LOCAL INFILE** statement used. Here is what has happened:

+   - We chose **author.csv**.
+   - The contents of this file were transferred over HTTP and received by the web server.
+   - The PHP component inside the web server saved this file in a work directory (here **/opt/php-upload-tmp/**) and gave it a temporary name.
+   - phpMyAdmin, informed of the location of this working file, built a **LOAD DATA LOCAL INFILE** command, and sent it to MySQL. Note that just one query was executed, which loaded many rows.
+   - The MySQL server read and loaded the contents of the file into our target table. It then returned the number of affected rows (2), which phpMyAdmin displayed in the results page.

# Web server upload directories

To get around cases where uploads are completely disabled by a web server's PHP configuration, or where upload limits are too small, phpMyAdmin can read upload files from a special directory located on the web server's file system. This mechanism is applicable for SQL and CSV imports.

We first specify the directory name of our choice in the $cfg['UploadDir'] parameter, for example, './upload'. We can also use the %u string, as described in Chapter 7, to represent the user's name.

Now, let's go back to the Import subpage. We get an error message:

**The directory you set for upload work cannot be reached.**

This error message is expected, as the directory does not exist. It is supposed to have been created inside the current phpMyAdmin installation directory. The message might also indicate that the directory exists, but can't be read by the web server. (In PHP safe mode, the owner of the directory and the owner of the phpMyAdmin-installed scripts must be the same.)

Using an SFTP or FTP client, we create the necessary directory, and can now upload a file there (for example book.sql) bypassing any PHP timeouts or upload maximum limits. Note that the file itself must have permissions that allow the web server to read it. In most cases, the easiest way is to allow everyone to read the file.