# Backing up website files

**Website files** are the files that you upload to your FTP directory. Most of them are delivered from the team of developers and have been copied to your web space before installation. In the `uploads` directory, you will find files, like `images`, that have been uploaded by the editors. If you have installed additional modules, then you will find files for these modules in the `modules` folder.

To backup files, you can download everything from your FTP to the local disk. However, depending on your connection and the total number of files, this can take a lot of time.

# Time for action – creating a backup with the module

If you would like to archive the files before downloading them, then you can use the **FileBackup** module.

+   1. In the admin console, select **Extensions | Module Manager**.
+   2. Click on the **Available Modules** tab, and look for the letter **F** above the alphabetical module list.
+   3. Find the **FileBackup** module, and click on the **Download & Install** link in the same line. If there are restrictions on your web hosting, then you will see the following error:
+   4. In the admin console, go to **Site Admin | File Backup**.
+   5. Click on the **Preferences** tab. In the admin area you see the following message:
+   6. Click **Save**. (This module tries to recognize the required path automatically). If the message does not disappear, then you have to ask your hosting provider what the path to `tar` binaries on your web hosting is and enter the path into the field **Path to tar binaries**.

> You cannot use this module, since you don't have the tar command installed on your server.

In this case, you will have to backup your website manually. If the installation of the module **FileBackup** was successful, then you can start configuring it.

> Please check and save the path to tar binaries before using FileBackup.