> You won't see PBMS in the storage engine choices of the **Operations** subpage for a table. PBMS is not a storage engine that can be associated with the structure of a table.

# Configuration

Some PBMS system variables have an impact on BLOB streaming behaviour, and can be defined in `config.inc.php`. More details about them on http://blobstreaming.org/documentation/#sysvar. The recommended values to be put in your server's definition are:

```
$cfg['Servers'][$i]['bs_garbage_threshold'] = 50;
$cfg['Servers'][$i]['bs_repository_threshold'] = '32M';
$cfg['Servers'][$i]['bs_temp_blob_timeout'] = 600;
$cfg['Servers'][$i]['bs_temp_log_threshold'] = '32M';
```

# Implementation limitations in phpMyAdmin

In the current 3.1.1 version of phpMyAdmin, a few limitations exist, with respect to using BLOB streaming. The first limitation has to do with the MySQL server. On a server which has just started, the `pbms_field_references` global MySQL variable may be set to **off**. As field references are used extensively for BLOB referencing, phpMyAdmin tries to set the parameter to on, and this can be done only by a user possessing the SUPER privilege. Therefore, such a user must log in once via phpMyAdmin to set this parameter to the appropriate value, and this will have to be repeated on each restart of the MySQL server.

The second limitation is related to the upload mechanism used to send data to the BLOB repository. The data has to travel first from your workstation to the web server, and then from the web server to the MySQL server. The first travel occurs via a normal HTTP upload. Please refer to Chapter 8, *Importing Structure and Data*, in the *Limits for the transfer* section, for more details on the requirements for uploading. To avoid being hindered by this limit, you can use the web server upload directory (also described in Chapter 8).

> An important note about the host name is due here: In `config.inc.php`, your server name in `$cfg['Servers'][$i]['host']` should be a **FQDN (fully-qualified domain name)**. It should not something like "localhost"; otherwise, the links used to retrieve streaming data will not work.