
## Saving the export file on the server

Instead of transmitting the export file over the network with HTTP, it is possible to save it directly on the file system of the web server. This could be quicker and less sensitive to execution time limits because the entire transfer from the server to the client browser is bypassed. Eventually, a file transfer protocol such as FTP or SFTP can be used to retrieve the file, as leaving it on the same machine would not provide good backup protection.

A special directory has to be created on the web server before saving an export file on it. Usually, this is a subdirectory of the main phpMyAdmin directory. We will use save_dir as an example. This directory must have special permissions. First, the web server must have write permissions for this directory. Also, if the web server's PHP component is running in safe mode, the owner of the phpMyAdmin scripts must be the same as that of save_dir.

On a Linux system, assuming that the web server is running as group apache, and the scripts are owned by a user "marc", the following commands would do the trick:

```
# mkdir save_dir
# chown marc.apache save_dir
# chmod g=rwx save_dir
```

> The proper ownership and permissions mask depend highly on the chosen web server and the SAPI (Server Application Programming Interface) (see http://en.wikipedia.org/wiki/Server_Application_Programming_Interface) used, which influences how directories and files are created and accessed. PHP could be using the scripts' owner as the accessing user, or the web server's user/group itself.

We also have to define the './save_dir' directory name in $cfg['SaveDir']. We are using a path relative to the phpMyAdmin directory here, but an absolute path would work just as well.

The Save as file section will appear with a new Save on server... section:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_163_0.png)

After clicking Go, we will get a confirmation message or an error message (if the web server does not have the required permissions to save the file).

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_164_0.png)

> For saving a file again, using the same file name, check the **Overwrite existing file(s)** box.

## User-specific save directories

We can use the special string, %u, in the $cfg['SaveDir'] parameter. This string will be replaced by the logged-in user name. For example, using

```
$cfg['SaveDir'] = './save_dir/%u';
```

This would give us an on screen choice, **Save on server in /save_dir/marc/ directory**. These directories (one per potential user) must exist and must bear the proper permissions, as already seen in the previous section.

## Memory limits

Generating an export file uses a certain amount of memory, depending on the size of the tables and on the chosen options. The $cfg['MemoryLimit'] parameter can contain a limit (in bytes) for the amount of memory used by the PHP script that is running. By default, the parameter is set to 0, meaning that there is no limit. Note that, if PHP has its safe mode activated, changing $cfg['MemoryLimit'] has no effect. Instead, the enforced limit comes from the memory_limit directive in php.ini.

## Summary

In this chapter, we examine the various ways to trigger an export—from the Database view, the Table view, or a results page. We also list the various available export formats, their options, the possibility of compressing the export file, and the various places where it might be sent.

In the next chapter, we'll have the opportunity of importing back our structure and data, provided the chosen format is supported by phpMyAdmin.

## 8 Importing Structure and Data

In this chapter, we will learn how to bring back exported data that we create for backup or transfer purposes. Exported data may also come from authors of other applications, and could contain the whole foundation structure of these applications and some sample data.

The current phpMyAdmin version (3.1) can import files containing MySQL statements (usually having a .sql suffix, but not necessarily so) and CSV files (comma-separated values, although the separator is not necessarily a comma) directly. Future versions might be able to import files in more formats. There is also an interface to the MySQL LOAD DATA INFILE statement, enabling us to load text files containing data, also called CSV. The binary field upload covered in Chapter 6 can be said to belong to the import family.

> Importing and uploading are synonyms in this context.

The import feature can be accessed from several panels:

- The **Import** menu available from the homepage, the Database view, or the Table view
- The **Import files** menu offered inside the **Query window** (as explained in Chapter 12)

A feature was added in version 2.11.0: an import file may contain the DELIMITER keyword. This enables phpMyAdmin to mimic the mysql command-line interpreter. The DELIMITER separator is used to delineate the part of the file containing a stored procedure, as these procedures can themselves contain semicolons.

The default values for the Import interface are defined in $cfg['Import'].

Before examining the actual import dialog, let's discuss some limits issues.

## Limits for the transfer

When we import, the source file is usually on our client machine; so, it must travel to the server via HTTP. This transfer takes time and uses resources that may be limited in the web server's PHP configuration.

Instead of using HTTP, we can upload our file to the server using a protocol such as FTP, as described in the Web Server Upload Directories section. This method circumvents the web server's PHP upload limits.

## Time limits

First, let's consider the time limit. In config.inc.php, the $cfg['ExecTimeLimit'] configuration directive assigns, by default, a maximum execution time of 300 seconds (five minutes) for any phpMyAdmin script, including the scripts that process data after the file has been uploaded. A value of 0 removes the limit, and in theory, gives us infinite time to complete the import operation. If the PHP server is running in safe mode, modifying $cfg['ExecTimeLimit'] will have no effect. This is because the limits set in php.ini or in user-related web server configuration file (such as .htaccess or virtual host configuration files) take precedence over this parameter.

Of course, the time it effectively takes depends on two key factors:

- Web server load
- MySQL server load

> The time taken by the file, as it travels between the client and the server, does not count as execution time because the PHP script only starts to execute after the file has been received on the server. Therefore, the $cfg['ExecTimeLimit'] parameter has an impact only on the time used to process data (like decompression or sending it to the MySQL server).

## Other limits

The system administrator can use the php.ini file or the web server's virtual host configuration file to control uploads on the server.

The upload_max_filesize parameter specifies the upper limit or the maximum file size that can be uploaded via HTTP. This one is obvious, but another less obvious parameter is post_max_size. As HTTP uploading is done via the POST method, this parameter may limit our transfers. For more details about the POST method, please refer to http://en.wikipedia.org/wiki/Http#Request_methods.

The memory_limit parameter is provided to avoid web server child processes from grabbing too much of the server memory – phpMyAdmin also runs as a child process. Thus, the handling of normal file uploads, especially compressed dumps, can be compromised by giving this parameter a small value. Here, no preferred value can be recommended; the value depends on the size of uploaded data. The memory limit can also be tuned via the $cfg['MemoryLimit'] parameter in config.inc.php, as seen in Chapter 7.

Finally, file uploads must be allowed by setting file_uploads to on. Otherwise, phpMyAdmin won't even show the Location of the textfile dialog. It would be useless to display this dialog, as the connection would be refused later by the PHP component of the web server.

## Partial imports

If the file is too big, there are ways in which we can resolve the situation. If we still have access to the original data, we could use phpMyAdmin to generate smaller CSV export files, choosing the Dump n rows starting at record # n dialog. If this were not possible, we will have to use a text editor to split the file into smaller sections. Another possibility is to use the upload directory mechanism, which accesses the directory defined in $cfg['UploadDir']. This feature is explained later in this chapter

In recent phpMyAdmin versions, the Partial import feature can also solve this file size problem. By selecting the Allow interrupt... checkbox, the import process will interrupt itself if it detects that it is close to the time limit. We can also specify a number of queries to skip from the start, in case we successfully import a number of rows and wish to continue from that point.

## Temporary directory

On some servers, a security feature called open_basedir can be set up in a way that impedes the upload mechanism. In this case, or for any other reason, when uploads are problematic, the $cfg['TempDir'] parameter can be set with the value of a temporary directory. This is probably a subdirectory of phpMyAdmin's main directory, into which the web server is allowed to put the uploaded file.
