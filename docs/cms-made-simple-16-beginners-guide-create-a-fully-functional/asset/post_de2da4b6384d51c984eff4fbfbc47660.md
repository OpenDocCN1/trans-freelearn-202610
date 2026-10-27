
- **Third-party modules:** Developers of additional modules release new versions, including new functionality and corrected bugs. Newer versions of the modules often assume that the newest version of CMS Made Simple is installed. Sometimes, the feature you need is already implemented in the last version of the module, but you are not able to install it due to the outdated version of CMS Made Simple.

- **Security releases:** Not only are errors corrected in new releases and new functions added, but from time-to-time, security holes are discovered. In this case, exploits are quickly spread throughout the internet. Exploits are generally sequences of commands that take advantage of a bug in order to take control over your system. If you do not upgrade, then your system becomes extremely vulnerable, your data could be damaged or lost, and nobody would take responsibility for it except you.

- **Stability and performance:** New releases often include improvements in performance. Even if you are not interested in the features included in new releases, your website can gain speed just by upgrading.

If a new version has been released, then you will see a message saying **You have 1 unhandled notification** in the admin console below the main menu. The message appears only once a day! Click on the plus sign beside the message, as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_325_0.png)

Let's see how to upgrade your installation to the newer version.

It is important to create a backup of your database and all the files on the web hosting before every upgrade (please refer to the section *Backup your website*). If something goes wrong, you can always return to the previous version and start upgrading again. Having no backup means having no possibility to go back in case of an emergency.

Open the link `http://cmsmadesimple.org` in the browser, and select **Downloads | File Releases** in the main menu on the top of the page. The list of releases is shown. If you are not sure what version of CMS Made Simple is currently installed on your web hosting, then open the admin console; you can see in the footer area at the very bottom of the page what version is used on your website.

In the downloads section, find the newest release, and choose the file named `cmsmadesimple-full-diff-[YOUR.CURRENT.VERSION]-[NEW.VERSION].tar.gz`. For example, to upgrade from version 1.6 to 1.6.5, you have to choose the file `cmsmadesimple-full-diff-1.6-1.6.5.tar.gz`. Save the file to your local disk and extract it.

If you cannot find the upgrade file from exactly your version to the current one, then you will have to download the last full version above the one installed (for example, 1.6) and proceed with these instructions using that version first.

Before copying the files to the web server, you should probably set up a **Site Down Message (Site Admin | Sitedown Settings)**. Now open an FTP browser, and upload all the files from the extracted folder to your web hosting. Overwrite the existing files and directories. You have to overwrite these existing files with the newer versions.

After uploading the new files, check if a folder called `/install` has been created in the folder where CMS Made Simple is installed. If so, then enter in your browser `http://www.yourdomain.com/install/upgrade.php` (substitute the domain with yours). This will automatically update your database to the newest version. Do not forget to delete the install folder after upgrade. If a folder named install was not created, then you have finished the upgrade at this point.

Check the version in the footer area of the admin console. It should display the new version now.

You then have to upgrade the modules. In the admin console, click on **Extensions | Module Manager**, and see if some modules have to be upgraded on the **Available Upgrades** tab.

![](img/b21e3dc5c94405bc7b3f102758ea770c_326_0.png)

If there are some upgrades shown, then look for the modules on the **Available Modules** tab, and click on the **Upgrade** link beside the module name.

> Sometimes, the notification for the new version is not displayed in the admin console. This is due to the security settings of your web hosting. Another possible way of being informed about a new version in this case is subscribing to the official RSS feed from the CMS Made Simple blog or (if you are a registered member in CMS Made Simple Forum) to subscribe to the board **Announcements**.

Remember that upgrading your system is not difficult if you do it regularly. Go with the development to get the best free support and to be sure that your system is secure and stable.

## Optimizing (performance tuning)

Like every system, CMS Made Simple can be optimized and tuned for the best performance. The tips described in this section can significantly speed up your system. However, I recommend applying these instructions only if you encounter any performance issues.

Before you start to optimize your system, you should know how the system performs without any optimization. CMS Made Simple has built-in performance measurement. If you open any page generated by CMS Made Simple in a browser and then see the source code of the page, you will find HTML comments at the end of every page, shown as follows:

```
<!-- 0.267839 / 38 / 5834456 / 6113588 -->
```

This comment can be broken into four parts:

1. The page was generated in 0.267839 seconds by CMS Made Simple.
2. 38 SQL queries were needed to generate pages.
3. 5834456 bytes of memory were used.
4. The peak memory usage was 6113588 bytes.

You should save this comment to control the amount of resources that can be saved using optimization tips, given in the following sections. Every time you make an optimization, compare the new numbers on the same page with the previous ones.

### Compression

This option allows the output from your server to be compressed before being sent to the browser. In simple words, the page is zipped, and instead of sending HTML, the server sends a much smaller ZIP file. The smaller the file, the faster the website can be loaded. The browser is able to decompress the zipped page automatically, so that neither you nor your visitor should care about it.

Compression can be turned on in the file config.php. Open the file and find the line:

```
$config['output_compression'] = false;
```

Turn on the compression by setting it to true:

```
$config['output_compression'] = true;
```

This may provide a significant performance increase on some sites. However, it is not needed for Apache servers that have mod_deflate enabled. Please contact your provider if you are not sure. Consider that enabling this feature will significantly increase the CPU and RAM usage on the server side, and this can even slow down the website if the server is very busy.

### Persistent connections

CMS Made Simple is based on the database. Therefore, for each page loaded in the browser, a connection to the database is needed. There are two concepts on how CMS Made Simple connects to the database. Normally, for each page, the new connection to the database has to be established. A persistent connection is a concept which is designed to improve performance for some applications. Instead of the connection being established for each page, it is opened once and then kept for the application's lifetime.

In the file config.php, you can define how CMS Made Simple should connect to the database. Open the file and search for the following line:

```
$config['persistent_db_conn'] = false;
```

Change the line to:

```
$config['persistent_db_conn'] = true;
```

This would ensure that the connection to the database is not closed after the page is loaded but is kept open. The next time the page is requested, the server checks whether there is an opened connection and uses it for the new page. In this way, the time for establishing the new database connection is saved.

> This setting is not allowed by all providers. Please contact your provider to figure out if you can use persistent connections on your web hosting.

### CSS in global settings and static CSS

Another place where you can save time is in the generation of the stylesheet for the page. If you open your page in the browser and see the page source, then you will see that the links to stylesheets are actually references to the PHP file stylesheet.php:

```
<link rel="stylesheet" type="text/css" href="http://yourdomain.com/stylesheet.php?cssid=59" />
```

However, it is not necessary to generate stylesheets with every page again and again. You can cache them when you are ready with the design of the website and do not plan to change it.
