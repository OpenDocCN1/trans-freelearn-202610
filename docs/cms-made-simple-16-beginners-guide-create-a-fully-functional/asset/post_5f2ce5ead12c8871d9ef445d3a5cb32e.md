
## Hiding admin directory

Every CMS has an admin URL. In CMS Made Simple, this URL is `http://www.yourdomain.com/admin`. This URL is defined by default, so that any experienced CMS Made Simple user can get the login screen to your admin console. The more information you reveal to the hacker, the more vulnerable your system is. In CMS Made Simple, you can easily change the URL of the admin console, so that one cannot guess where the "entrance" is.

Open your FTP browser and rename the folder `admin` to something that cannot be guessed easily (for example, `private4me` or `hide2safe`). Then, open the file `config.php` with any FTP browser of your choice and find the line

```
```php
$config['admin_dir'] = "admin";
```
```

Change the line to the name you have given to the folder `admin`. If your admin directory is now named `private4me`, then change the line shown as follows:

```
```php
$config['admin_dir'] = "private4me";
```
```

From now on, the URL to your admin console will be `http://www.yourdomain.com/private4me`. If you enter the standard admin URL in the browser now, then you will get the error 404 (Page not found) instead of the login screen. In combination with a strong username and password, this would give additional protection to your admin area. An attacker must guess the admin URL first, before he tries to manipulate the username or password information.

When you rename the `admin` folder to something else, the default verification checksum file will not work. This file will still look for the `admin` folder, will not find it, and then give you an error for each file with this no longer existing folder. Therefore, directly after renaming of the `admin` folder, create a new custom verification file (**Site Admin | System Verification**), and save it to your local disk. In this way, you hold the current status of the system, considering the renamed admin directory.

When upgrading CMS Made Simple, before copying upgrade files to your current installation, rename your folder back to `admin`, perform the upgrade, and restore the custom name of the folder again.

## File permissions

If your installation is on Linux server (please check **Site Admin | System Information, Server Operating System** if you are not sure), then you should know how file permissions can be configured to secure your system. A Linux server needs to know two things about the file:

- What can be done to a file?
- Who can do it with the file?

The first question can be broken down to three actions: **read**, **write**, and **execute**. Reading a file means just opening the file and looking at its contents. Writing means changing, replacing, or deleting the file. The last option checks if programs or scripts are allowed to be executed.

The second question can be broken into three types of user: **owner**, **group**, and **public**. The owner of the file is the user who has initially created it. The group is not important for our needs and will be treated the same way as public. Public is every other user on our server who is not the owner of the file.

With Linux permissions, we combine the preceding information and say who is allowed to do what with each file. You can imagine a permission matrix shown as follows:

| Permission | Owner | Group | Public |
|------------|-------|-------|--------|
| **Read**   | 4     | 4     | 4      |
| **Write**  | 2     | 2     | 2      |
| **Execute**| 1     | 1     | 1      |

You see that there are theoretically 27 possible combinations for the permissions of only one file. Any combination can be expressed by the sum of the numbers in the preceding table and changed with the Linux command **chmod** (change mode). However, you do not need to know exactly how this command is executed as every FTP browser lets you define the permissions intuitively.

Let's see and understand some examples for the permission sets:

- **777** (1+2+4 for each column) means that everybody (all three groups) is allowed to read, write, and execute the file.
- **755** means that the owner is allowed to do everything with the file, but group and public users are only allowed to read and execute the file. They are not allowed to change it.
- **644** means that the owner is allowed to execute and read the file, others are only allowed to read it.

There is a very important file in your installation that is worth securing with Linux permissions additionally. This file contains the database’s connection data and is called `config.php`. It is like the heart of your installation.

While installing or upgrading, the file must be writable at least by the owner of the file, so its permissions must be set to at least **644**. As soon as the installation or upgrade is finished, you should lower the permissions of this file to **444** (everyone can read the file, but nobody can execute or change it).

In an FTP browser like FileZilla, you can right click on the file and select the option **File permissions**. The next dialog looks like the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_335_0.png)

Generally, avoid setting any files to **777** if it is not absolutely needed. This permission allows the reading, changing, and deleting of the file by every user on your server.

## CMS Made Simple and server version

In the default template of CMS Made Simple, the version is shown in the footer area as follows:

![](img/b21e3dc5c94405bc7b3f102758ea770c_335_1.png)

This information is provided in the template with the Smarty plugin `{cms_version}`. Delete it and do not use it in your templates at all. You should avoid revealing the exact CMS Made Simple version you use. The reason for this recommendation is the following scenario.

You use an actual version of CMS Made Simple. One day, a security issue is discovered in this version. The development team of CMS Made Simple reacts and publishes a security release. If you do not upgrade your system immediately, your website would be extremely vulnerable as everybody knows exactly what version you use on the website.

For the same reason, delete the doc folder from your installation. This folder contains the file `changelog.txt` which will reveal the actual version of CMS Made Simple as well. A malicious person just has to enter in the address bar of the browser the following line:

```
http://www.yourdomain.com/doc/changelog.txt
```

> Backup all your files and your database again and again. If something is damaged or lost, then you can save your time and nerves by replacing the damaged version with the last backup.

## Troubleshooting

While designing and configuring websites, you will surely encounter some issues. There is a solution for each problem if you know how and where to get help.

If you install CMS Made Simple with sample content and templates, then you will get the well documented content pages, templates, and stylesheets with a new installation of CMS Made Simple. They are an invaluable way of introducing yourself to the application and getting to know how it works. Read all these pages thoroughly and save them for future reference.

## CMS Made Simple Wiki

The purpose of this Wiki is not only to allow the CMS Made Simple community to collaborate in the documentation process, but to also give you an appropriate and easy way to find information. This documentation is made by CMS Made Simple users and partly covers more languages than just English. However, a major part of the Wiki still remains in English. You will find the documentation on http://wiki.cmsmadesimple.org.

You can search this documentation to find answers to your questions. A search button is at the top-right of every page for your convenience. You can read this manual page-by-page or pick the pages you like. In the Table of Contents at the right-hand side, you can see where you are (the title of the page you are reading is black).

In the admin console of CMS Made Simple, you will see the Community Help link at the top-right of every page (below the main navigation). This link will take you directly to the Wiki page dealing with the function or the module you currently open in the admin console. However, there are still pages that are not written yet.

The more users participate in writing documentation, the more answers can be found in it. CMS Made Simple really needs everyone's assistance to make the documentation accurate, user friendly, and understandable. If you want to help, then please register as an editor on the Wiki site and start to help improving and enhancing the documentation for your own reference as well.
