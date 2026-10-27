# # Hiding admin directory

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

# # File permissions

If your installation is on Linux server (please check **Site Admin | System Information, Server Operating System** if you are not sure), then you should know how file permissions can be configured to secure your system. A Linux server needs to know two things about the file:

+   - What can be done to a file?
+   - Who can do it with the file?