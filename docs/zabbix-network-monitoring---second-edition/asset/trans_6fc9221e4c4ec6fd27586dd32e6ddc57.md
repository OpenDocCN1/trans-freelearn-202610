

Feel free to experiment with other parameters, such as stop and restart — it should be obvious what these two do.

You can verify whether services are running with the status parameter. For a service that is not running, you would get the following:

```
# service zabbix-server status
Checking for service Zabbix server daemon
unused
```

A running service would yield the following:

```
# service zabbix-agentd status
Checking for service Zabbix agent daemon
running
```

> On some distributions, this might return more verbose output, including all of the running processes.

Some distributions might have another parameter called probe. This will check whether the corresponding service has been restarted since the last configuration file changes.

If it has been restarted, no output will be produced. If the service has not been restarted (thus possibly missing some configuration changes), the reload string will be output.

While it's nice to have Zabbix processes running, it's hardly a process one expects to do manually upon each system boot, so the server and agent should be added to your system's startup sequence. This is fairly distribution specific, so all possible variations can't be discussed here. With RHEL or CentOS, a command like this should help:

```
# chkconfig --level 345 zabbix-agent on
# chkconfig --level 345 zabbix-server on
```

This will add both services to be started at runlevels 3, 4, and 5. For OpenSUSE, the following should work:

```
# chkconfig -s zabbix-server 35
# chkconfig -s zabbix-agentd 35
```

First, you have to decide where the frontend code has to go. Most distributions that package web servers use /srv/www/htdocs or /var/www. If you compiled the Apache web server from source, it would be /usr/local/apache2/htdocs (unless you manually changed the prefix or installed an older Apache version). We will place the frontend in a simple subdirectory, zabbix.

Assuming you have Apache distribution packages installed with the web root directory at /srv/www/htdocs, placing the frontend where it is needed is as simple as executing the following as the root user:

```
# cp -r frontends/php /srv/www/htdocs/zabbix
```

## Using the web frontend configuration wizard

The web frontend has a wizard that helps you to configure its basics. Let's go through the simple steps it offers.

Now, it's time to fire up a browser and navigate to Zabbix's address: http://<server_ip_or_name>/zabbix. It should work just fine in the latest versions of most browsers, including Firefox, Chrome, Safari, Opera, Konqueror, and Internet Explorer.

### Step 1 – welcome

If everything has been configured properly, you should be greeted by the installation wizard:

![](img/f5313a804532fabacc8d07cc8607eeba_49_0.png)

## Getting Started with Zabbix

If you are not, there are several things that could have gone wrong.

If the connection fails completely, make sure Apache is started up and there is no firewall blocking access. If you see a blank page or some PHP code, make sure that PHP is properly installed and configured to parse files ending with the .php extension through the AddType application/x-httpd-php directive. If you see a file and directory listing instead of the installation wizard, make sure you have added index.php to the DirectoryIndex directive. If these hints do not help, check the PHP documentation at http://www.php.net/manual/en/.

This screen doesn't offer us much to configure, so just click on **Next step**.

### Step 2 – PHP prerequisites

In this step, the installation wizard checks PHP-related prerequisites. If you are lucky, all will have been satisfied, and you will be greeted with all green entries:

![](img/f5313a804532fabacc8d07cc8607eeba_50_0.png)

If so, just click on the **Next step** button to continue to *Step 3*.

However, more often than not, one or more entries will have a red **Fail** warning listed next to them. This is where things get more interesting. Problems at this point fall into two categories: **PHP installation** or **configuration**.

Entries such as **PHP version**, **PHP databases support**, **PHP bcmath**, **PHP mbstring**, **PHP gd**, **PHP gd PNG/JPEG/FreeType support**, and others that are not listed as an **option** are PHP installation problems. To solve these, either install the appropriate distribution packages (sometimes called **php5-bcmath**, **php5-gd**, **php5-mysql**, and so on), or recompile PHP with the corresponding options.

**PHP option "memory_limit"**, **PHP option "post_max_size"**, **PHP option "upload_max_filesize"**, **PHP option "max_execution_time"**, **PHP option "max_input_time"**, and **PHP time zone** are configuration issues that are all set in the php.ini configuration file. This file is usually located at `/etc/php5` or similar for distribution packages and `/usr/local/lib` for PHP source installations. Set the following options:

```
memory_limit = 128M
post_max_size = 16M
max_execution_time = 300
max_input_time = 300
upload_max_filesize = 2M
```

> 💡 The code bundle for the book is also hosted on GitHub at https://github.com/PacktPublishing/Zabbix-Network-Monitoring-Second-Edition. We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

For the time zone, set the `date.timezone` option to a time zone that best matches your environment. The default for Zabbix is Europe/Riga, and you can see valid options at `http://www.php.net/manual/en/timezones.php`.

Make sure you restart Apache after changing the PHP configuration file. If you can't find `php.ini`, or you make changes but the installation wizard does not pick them up, create a file named `test.php` in the `htdocs` directory with only this content:

```
<?php phpinfo() ?>
```

Navigate to this file using your browser and check the value for a **Configuration File (php.ini) Path** entry — this is where you should look for `php.ini`.

Once everything is fixed, click on the **Next step** button to continue.

### Step 3 – database access

Remember the database we created earlier? That's the information we'll supply here:

![](img/f5313a804532fabacc8d07cc8607eeba_52_0.png)

We already configured database credentials for the Zabbix server, but the Zabbix frontend uses a different configuration file. The default Database type, Database host, and Database port values should work for us. Set both Database name and User to zabbix. If you have forgotten the Password, just look it up or copy it from zabbix_server.conf. After entering the data, click on the Next step button. If all the information is correct, the wizard should proceed to the next step.

### Step 4 – Zabbix server details

The next screen lets you specify the Zabbix server's location:

![](img/f5313a804532fabacc8d07cc8607eeba_53_0.png)

The defaults for the host and port are suitable for us, but we could benefit from filling in the Name field. The contents of this field will be used for page titles and a label in the upper-right corner of the Zabbix interface—this could be really handy if we had multiple Zabbix installations. Feel free to enter any name here, but for this book, we'll call the server Zabbix One. When you're done, it's over to the Next step again. The next screen is a summary of the choices made in the previous screens.

### Step 5 — summary

If you left the defaults where appropriate and your database connection test was successful, it should be safe to continue by clicking on **Next step**:

![](img/f5313a804532fabacc8d07cc8607eeba_54_0.png)

### Step 6 — writing the configuration file

It is quite likely that in the next screen, you will be greeted with failure:

![](img/f5313a804532fabacc8d07cc8607eeba_55_0.png)

The installation wizard attempted to save the configuration file, but with the access rights that it has, it should not be possible. Previous versions of Zabbix explained two alternatives for proceeding. Unfortunately, Zabbix 3.0 has lost the explanation for one of those. The two possible solutions are as follows:

- 1. Click on **Download the configuration file** and manually place this file in the `htdocs/zabbix/conf` directory.
- 2. Make the `htdocs/zabbix/conf` directory writable by the web server user (execute as root). Use these commands:

```
```bash
# chown <username> /path/to/htdocs/zabbix/conf
# chmod 700 /path/to/htdocs/zabbix/conf
```
```

Obviously, we need to insert the correct username and directory in these commands. Remember, common locations are `/srv/www/htdocs` and `/usr/local/apache2/htdocs`—use the one you copied the Zabbix frontend code to. Common users are `wwwrun`, `www-data`, `nobody`, and `daemon`—you can find out which one the correct user is for your system by running this:

```
```bash
$ ps aux | grep http
```
```

