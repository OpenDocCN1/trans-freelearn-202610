
## Getting system information

As the website administrator, you should know how to get the system information for your website or your customer's installation. This information helps you to understand how the server, where CMS Made Simple is installed and configured, gives you the first indicators when you encounter problems while running CMS Made Simple or any of its modules.

In the admin console, click Site Admin | System Information. The information page is divided into three sections:

- CMS Install Information
- PHP Information
- Server Information

In the first section you see the version of CMS Made Simple installed and all enabled modules with their versions as well.

![](img/b21e3dc5c94405bc7b3f102758ea770c_313_0.png)

Failures should be corrected as soon as possible. Warnings can be ignored if you do not have any issues when running CMS Made Simple, but you should correct them if something does not work as expected. In the following table, you will find some required and recommended settings for CMS Made Simple. Required settings are mandatory. Recommended settings can make your system run smoother and solve some issues.

| Setting | Minimum and recommended values |
|---|---|
| **Current PHP Version (phpversion)** | 5.0.4 or greater |
| **GD version (gd_version)** | version 2 |
| **Magic quotes in runtime (magic_quotes_runtime)** | On (True) |
| **PHP Effective Memory Limit (memory_limit)** | minimum 16 MB<br>recommended 24 MB or greater |
| **Maximum Execution Time (max_execution_time)** | minimum 30<br>recommended 60 or greater |
| **PHP Safe Mode (safe_mode)** | Off (False) |
| **Session Save Path (session_save_path)** | This path should be writable (green tick) |
| **PHP register_globals (register_globals)** | Off (False) |
| **Maximum Post Size (post_max_size)** | minimum 2M<br>recommended 10M or greater |
| **Maximum Upload Size (upload_max_filesize)** | minimum 2M<br>recommended 10M or greater |

The third section on the **System Information** page gives you an overview of your server's configuration. If you are the website administrator, then you should be able to answer at least three questions about your system:

- What operating system is used on your server?
- What server software (and which version) is installed on your server?
- What database (and which version) is used for a CMS Made Simple installation?

All these questions can be answered if you look at the information given here thoroughly.

![](img/b21e3dc5c94405bc7b3f102758ea770c_316_0.png)

From the preceding screenshot, I can say that this installation of CMS Made Simple is installed on Linux (Server Operating System), Apache server version 2 is installed (Server Software), and this server uses the database MySQL 5.0.32 (Server Database + Server Database Version).

It is important to know how your server is configured to be able to get help if you encounter issues. There are a lot of varieties of applications' versions and configurations, so nobody can guess exactly how your individual hosting is configured. Keep that in mind while getting free help from the community.

## Backing up your website

Backing up means making copies of your files and database. You need to backup regularly in case something goes wrong on your web hosting. Your data could be lost due to some hosting issues or through webmaster mistakes. Therefore, it is important to have backup files to be able to restore your website.

To backup a website made with CMS Made Simple, it is not sufficient just to copy all the files found on the server with FTP. Those files are only a small fraction of your website. The most important part is the database where all your text and designs (except image files) are stored.

In CMS Made Simple, you have two modules (one is used for file backups and one for database backups). You would probably not be able to use the modules on free web hosting or on the hosting with strong restrictions. To check if an easy backup of your website with the following described modules is possible, try to install the first module to backup files.

## Backing up website files

**Website files** are the files that you upload to your FTP directory. Most of them are delivered from the team of developers and have been copied to your web space before installation. In the `uploads` directory, you will find files, like `images`, that have been uploaded by the editors. If you have installed additional modules, then you will find files for these modules in the `modules` folder.

To backup files, you can download everything from your FTP to the local disk. However, depending on your connection and the total number of files, this can take a lot of time.

### Time for action – creating a backup with the module

If you would like to archive the files before downloading them, then you can use the **FileBackup** module.

1. In the admin console, select **Extensions | Module Manager**.
2. Click on the **Available Modules** tab, and look for the letter **F** above the alphabetical module list.
3. Find the **FileBackup** module, and click on the **Download & Install** link in the same line. If there are restrictions on your web hosting, then you will see the following error:

   > You cannot use this module, since you don't have the tar command installed on your server.

   In this case, you will have to backup your website manually. If the installation of the module **FileBackup** was successful, then you can start configuring it.

4. In the admin console, go to **Site Admin | File Backup**.
5. Click on the **Preferences** tab. In the admin area you see the following message:

   > Please check and save the path to tar binaries before using FileBackup.

6. Click **Save**. (This module tries to recognize the required path automatically). If the message does not disappear, then you have to ask your hosting provider what the path to `tar` binaries on your web hosting is and enter the path into the field **Path to tar binaries**.
7. Switch to the **Backup Files** tab.
8. Click on **Backup Files**.
9. Wait until the complete list of folders is displayed and you see the message **Task completed** at the end of the list. Below this message, you will see the backup file containing all your website files.
10. Click on the backup file, and save it to your local disk.

![](img/b21e3dc5c94405bc7b3f102758ea770c_318_0.png)

### What just happened?

You have created an archived backup of all website files and have saved the archive to your local disk. You should create a new backup file and save it separately from the current installation. For example, on the local disk, anytime you install a new module, upgrade the whole system or one module, or upload new images. Do it regularly. You will save a lot of time in case of emergency.

All backup files will also be saved in your installation. You will find them using any FTP browser (for example, **FileZilla**) in the folder `/admin/backups` on your web hosting.

> Make sure you delete the backup files after downloading to your local disk. Otherwise, anybody could go to `http://www.yourdomain.com/admin/backups/` and download your backup, containing the file `config.php` with your database settings and password.

It is recommended to clear the cache folder before creating a file backup as CMS Made Simple creates a lot of temporary files to speed up the website. These files must not be saved with the backup as they will be generated automatically once CMS Made Simple is installed and running. To clear the cache folder in the admin console, click on **Site Admin | Global Settings**, select the **Advanced Setup** tab, and click the **Clear** button below the **Clear Cache** label.

In fact, the best solution would be to set up some automation if the files are changed frequently. You should contact your ISP to determine whether it is possible to backup the file automatically.
