
Please follow the installation instructions carefully as they differ from the installation process for many other Joomla! extensions.

Proceed with the following steps to install CiviCRM in Joomla!:

- 1. Upload the installation package to your website using FTP software. We recommend that you upload the package to /YOUR_JOOMLA_ROOT/tmp/. This folder is created by Joomla! and is the most appropriate place to store installation packages.
- 2. Unpack the installation package. You may do this using the command line or a web-based file browser hosted on your server (such as cPanel File Manager). There are various extensions available for Joomla! that let you work with the filesystem from within Joomla!, which may also be helpful. If you have difficulty unpacking the installation package online, you may need to unpack it on your local computer and use FTP software to upload the unpacked set of files to the /YOUR_JOOMLA_ROOT/tmp/ directory. After uploading and unpacking, you should see a /com_civicrm/ folder inside the /YOUR_JOOMLA_ROOT/tmp/ directory.
- 3. Browse to Extensions | Install/Uninstall in the Joomla! administrative interface. Using the Install from Directory option, enter the full file location of the unpacked /com_civicrm/ directory. In most cases, Joomla! will pre-fill that field with the proper location of the /YOUR_JOOMLA_ROOT/tmp/ directory and you will only need to add com_civicrm.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_91_0.png)

- 4. After clicking on **Install**, you should be directed to a screen confirming a successful installation. Click on the link provided to enter CiviCRM and confirm that it is installed and working.

If you experience problems installing CiviCRM, review the *Installation troubleshooting* section for a general review of possible server and environment-related issues. With Joomla! installation, the most common problem arises from unfamiliarity with the **Install from Directory** method we just described. Most Joomla! users are used to the more common **Browse and Install** method of installing extensions.

The two critical things to remember are as follows:

- The CiviCRM installation package must be unpacked on the server. You should be referencing a folder location—not the `.zip` file you downloaded from www.civicrm.org.
- The folder location must be the full filesystem path—not the path relative to your web directory. For example, it may look something like `/home/user/public_html/tmp/com_civicrm/`. If you received the following error message: **Please enter a package directory**, it is because you are not referencing the correct path to the unpacked installation files.

If you continue to experience problems during installation, visit the CiviCRM forum dedicated to Joomla! installation: `http://forum.civicrm.org/index.php/board,7.0.html`.

## Installation in Drupal

CiviCRM uses an automated installer to configure the software for use in Drupal. The following directions address the process of setting up an implementation of CiviCRM within your existing Drupal installation.

Drupal supports using a single codebase to operate multiple websites (see `http://drupal.org/getting-started/6/install/multi-site`). Site-specific data is stored in site-specific directories under `/path_to_docroot/sites/`, with each site's domain name typically used as the name of its subdirectory. This site-specific information includes the database name, unique to each site, and other connection information stored in the `/path_to_docroot/sites/yoursite.org/settings.php` file. CiviCRM supports this multi-site shared codebase and separate databases approach transparently during the installation process. Make sure to create a separate CiviCRM database for each site before starting the installation process.

Although this multi-site approach is still very popular, there are some countervailing trends in favor of single site installs. There are dependency, complexity, and timing issues when attempting to upgrade multiple sites sharing a codebase in a single directory simultaneously. More importantly, Drush (http://drupal.org/project/drush) and Aegir (http://groups.drupal.org/hostmaster2) have made it increasingly easier to manage a single codebase for multiple single site installs using multiple copies of the same code files.

The CiviCRM installation procedures in the following sections work if your Drupal site is either a single site or a multiple site configuration, as we just described. Different procedures are required if you want to use advanced techniques for sharing database tables between different Drupal sites in order to provide a single sign-on capability (visit http://thedrupalblog.com/setting-multi-site-drupal-6-installation-shared-databases-and-single-sign) or for sharing a database across several CiviCRM sites in order to share contacts and/or administrators between sites (visit http://wiki.civicrm.org/confluence/display/CRMDOC/Multi+Site+Installation).

Depending on your skill and comfort level, you can choose to install CiviCRM in one of the following two ways. The first one uses a browser and a client application for FTP. The second one uses SSH, a secure command line shell, and may be faster and more convenient for those with basic Linux skills.

### Browser/FTP procedure

Before you begin the installation process, take note of the database settings for your Drupal installation. If you do not know the values, you can find them at /path_to_docroot/sites/default/settings.php (the default location).

Locate the $db_url variable:

```
$db_url = 'mysqli://dbuser:dbpassword@localhost/drupal_dbname';
```

Here:

- Database server: localhost
- Drupal database name: drupal_dbname
- Database user name: dbuser
- Database user password: dbpassword

Older sites may still be using the mysql driver, rather than mysqli:

```
$db_url = 'mysql://dbuser:dbpassword@localhost/drupal';
```

If this includes your site, you should definitely consider upgrading to mysqli. First introduced to support MySQL 4.1 features, mysqli is now very stable and well-supported in PHP 5.x, and provides performance improvements over the mysql driver. Changing the driver specification from mysql to mysqli in the $db_url is all that is required for Drupal to gain these benefits.

You may now begin the installation process. Follow these steps:

- 1. If you haven't done so already, download the latest stable CiviCRM installation tarball for Drupal to your local machine from http://sourceforge.net/projects/civicrm/files/ (using the Download Now! button is the best way to ensure you get the right tarball).
- 2. Upload the CiviCRM installation package from your local machine to the /path_to_docroot/sites/all/modules/ directory in your Drupal installation. Installing CiviCRM and other modules under this directory will keep the files separate from core Drupal modules and facilitate any future Drupal core upgrades. (If you don't want CiviCRM to be available to other sites running off this codebase, you can create a modules subdirectory under your sites directory at /path_to_docroot/sites/default or /path_to_docroot/sites/yourdomain.org and install it there instead.) If necessary, adjust the permissions for the /path_to_docroot/sites/ directory to provide write access to the account that is running the web server process (and return it to the original permissions when complete). This can normally be done using your FTP or SFTP client applications, or by using the following command:

  ```
  $ cd /path_to_docroot/sites/ ; chmod -R 666 ./*
  ```

- 3. Unpack the installation package. You should now have the following directory in your site: /path_to_docroot/sites/all/modules/civicrm/ with other subdirectories contained within it.
- 4. Create a separate MySQL database for CiviCRM using an appropriate tool like phpMyAdmin or your hosting panel. The installer will create a new database for CiviCRM automatically if you enter a database name in the next step that doesn't already exist on your database server and the database user you enter has permission to create databases. We don't recommend granting that privilege to database accounts that don't need it, such as the ones used by your Drupal site.
