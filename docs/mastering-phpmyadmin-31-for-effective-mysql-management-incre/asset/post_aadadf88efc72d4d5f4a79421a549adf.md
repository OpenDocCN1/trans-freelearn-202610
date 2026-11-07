
The advanced features include:

- Field-level comments
- Foreign keys (with or without InnoDB)
- Browse foreign table
- Bookmarks of queries
- Data dictionary
- PDF relational schema and dictionary
- SQL queries history
- Connection to MySQL, using either the traditional `mysql` extension or the new `mysqli` extension (in PHP 5)
- Character-set support for databases, tables, and fields (with MySQL 4.1)
- Column contents transformation based on MIME type
- Visual Designer for relations
- Support for MySQL 5.0 features — views, procedures, triggers, profiling
- Support for MySQL 5.1 features — events and partitions
- Support for special storage engines — Maria, PBXT, and PBMS (BLOB streaming)
- Support for Swekey hardware authentication USB key
- Theme management to customize the look of the interface

The server administration features consist of:

- User and privileges management
- Database privileges check
- Verify server's runtime information and obtain configuration hints
- Full server export

## Installing phpMyAdmin

It's time to install the product and to configure it minimally for first-time use.

Our reason for installing phpMyAdmin could be one of the following:

- Our host provider did not install a central copy
- Our provider installed it, but the version installed is not current
- We are working directly on our enterprise's web server

## Required information

Some host providers offer an integrated web panel where we can manage accounts, including MySQL accounts, and also a file manager that can be used to upload web content. Depending on this, the mechanism we use to transfer phpMyAdmin to our webspace will vary. We will need some specific information (listed below) before starting the installation:

- The web server's name or address. Here, we will assume it to be www.mydomain.com.
- Our web server's account information (username, password). This information will be used either for FTP or SFTP transfer, SSH login, or web control panel login.
- The MySQL server's name or address. Often, this is localhost, which means that it is located on the same machine as the web server. We will assume this to be mysql.mydomain.com.
- Our MySQL server's account information (username, password).

## System requirements

The up-to-date requirements for a specific phpMyAdmin version are always stated in the accompanying Documentation.html. For phpMyAdmin 3.1, the minimum PHP version required is PHP 5.2 with session support and the Standard PHP Library (SPL). Moreover, the web server must have access to a MySQL server (version 5.0 or later) – either locally or on a remote machine. It is strongly recommended that the PHP mcrypt extension be present for improved performance in cookie authentication mode (more on this in Chapter 2). In fact, on a 64-bit server, this extension is required.

On the browser side, cookie support must be activated, whatever authentication mode we use.

## Downloading the files

There are various files available in the Download section of http://www.phpmyadmin.net. There might be more than one version offered here and it is always a good idea to download the latest stable version. We only need to download one file, which works regardless of the platform (browser, web server, MySQL, or PHP version). For version 3.1, there are two groups of files – english and all-languages. If we need only the English interface, we can download a file containing "english" – for example, phpMyAdmin-3.1.0-english.zip. On the other hand, if we have the need for at least one other language, a choice containing "all-languages" would be appropriate.

If we are using a server supporting only PHP4—for which the PHP team has discontinued support since December 31, 2007—the latest stable version of phpMyAdmin is not a good choice for download. I recommend using version 2.11.x, which is the latest branch that supports PHP4.

The files offered have various extensions: .zip, .tar.bz2, .tar.gz, .7z. Download a file having an extension for which you have the corresponding extractor. In the Windows world, .zip is the most universal file format, although it is bigger than .gz or .bz2 (common in the Linux/Unix world). In the following examples, we will assume that the chosen file was phpMyAdmin-3.1.1-all-languages.zip.

After clicking on the appropriate file, we will have to choose the nearest mirror site. The file will start to download, and we can save it on our computer.

## Installation procedure

The next step depends on the platform you are using. The following sections detail the procedures for some common platforms. You may proceed directly to the relevant section.

## Installation on a remote server using a Windows client

Using the File explorer, we double-click the phpMyAdmin-3.1.1-all-languages.zip file we just downloaded on the Windows machine. A file extractor should start, showing us all the scripts and directories inside a main phpMyAdmin-3.1.1-all-languages directory.

Use whatever mechanism your file extractor offers to save all the files, including subdirectories, to some location on your workstation. Here, we have chosen c:\. Therefore, a c:\phpMyAdmin-3.1.1-all-languages directory has been created for extraction.

Now, it's time to transfer the entire directory structure c:\phpMyAdmin-3.1.1-all-languages to the web server in our webspace. We use our favorite FTP software or the web control panel for the transfer.

The exact directory under which we transfer phpMyAdmin may vary. It could be our public_html directory or another directory where we usually transfer web documents. For further instructions about the exact directory to be used or the best way to transfer the directory structure, we can consult our host provider's help desk.

After the transfer is complete, these files can be removed as they are no longer needed on our Windows client.

## Installation on a local Linux server

Let's say we chose phpMyAdmin-3.1.1-all-languages.tar.gz and downloaded it directly to some directory on the Linux server. We move it to our web server's document root directory (for example, /var/www/html) or to one of its subdirectories (for example, /var/www/html/utilities). We then extract it with the following shell command or by using any graphical file extractor that our window manager offers:

```
tar -xzvf phpMyAdmin-3.1.1-all-languages.tar.gz
```

We must ensure that the permissions and ownership of the directory and files are appropriate for our web server. The web server user or group must be able to read them.

## Installation on local Windows servers (Apache, IIS)

The procedure here is similar to that described in the Installation on a remote server using a Windows client section, except that the target directory will be under our DocumentRoot (for Apache) or our wwwroot (for IIS). Of course, we do not need to transfer anything after modifications are made to config.inc.php (described in the next section), as the directory is already on the webspace.

Apache is usually run as a service. Hence, we have to ensure that the user under which the service is running has normal read privileges to access our newly created directory. The same principle applies to IIS, which uses the IUSR_machinename user. This user must have read access to the directory. You can adjust permissions in the Security/permissions tab of the directory's properties.

## Configuring phpMyAdmin

Here, we learn how to prepare and use the configuration file containing the parameters to connect to MySQL, and which can be customized as per our requirements.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_35_0.png)

> Before configuring, we can rename the directory phpMyAdmin-3.1.1-all-languages to something like phpMyAdmin, phpmyadmin, admin, or something easier to remember. This way, we and our users can visit an easily remembered URL to start phpMyAdmin. We can also use a symbolic link if our server supports this feature.
