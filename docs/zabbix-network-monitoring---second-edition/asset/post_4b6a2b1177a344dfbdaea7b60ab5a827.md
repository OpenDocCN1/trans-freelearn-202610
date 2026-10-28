
## Troubleshooting

Installing and configuring Zabbix can happen without a hiccup for one user and with a constant stream of problems for another. The reasons for the problems can differ from user to user – buggy libraries, bad distribution packaging, unintuitive configuration in Zabbix, or maybe even an occasional bug in Zabbix itself. Here, we will look at common problems new users experience when performing various tasks:

- Setting up the initial Zabbix installation
- Working with the web frontend
- Monitoring different devices
- Configuring thresholds and alerting

If you face a case that is more complicated than that, we will continue with more detailed debugging instructions, including:

- The Zabbix log file format
- Reloading the server and proxy configuration cache
- Controlling running daemons
- Observing the work performed by individual daemon processes

## Chapter introduction

This chapter caused a bit of mental agony. Having worked with Zabbix since 2001, there were quite a lot of potential problem cases to describe. I'd wake up in the middle of the night with an idea of a brilliant problem and solution to include, only to have forgotten it by the time morning came. But this would have been a never-ending chapter then. A compromise was needed, and reluctantly accepted. This chapter does not aim to help you with every single problem you will ever encounter in your life—or with Zabbix. It tries to help with more common issues and give you some hints and starting points for further debugging.

## Common issues

Here are a few issues you might face:

### Installation

There are several common stumbling blocks in the installation process, some caused by well-hidden factors.

### Compilation

- Q: I am trying to compile Zabbix on a 64-bit system. I have the corresponding development packages installed, but Zabbix claims they are not present.
- A: Double-check that the 64-bit development packages are installed, not just the 32-bit ones.
- Q: I am trying to compile Zabbix from an SVN checkout, but the configuration script fails with this error:
  `syntax error near unexpected token `IKSEMEL,iksemel,'`
- A: Install the `pkg-config` package and rerun the commands to generate the configuration script.
- Q: I am trying to compile Zabbix, but it fails.
- A: It is useful to reduce the number of possible causes. Verify that you are not compiling with `--enable-static`, which is known to cause compilation problems. If compilation fails without that flag, check the `config.log` file contents in the source directory. It often contains exact error details.

## Frontend

- **Q:** I have installed the Zabbix frontend. What's the default username and password?
- **A:** The username is Admin, and the password is zabbix.
- **Q:** I'm setting up Zabbix from an SVN checkout. When I switch languages in the frontend, nothing happens.
- **A:** In the frontend directory, in the locale subdirectory, there's a `make_mo.sh` script. It compiles the needed mo files out of the translation source po files—run it. Note that it will need Gettext tools, and the webserver might have to be restarted afterwards.

## Backend

- **Q:** Zabbix is working correctly, but some/all graphs are not displayed.
- **A:** Refer to the Apache error log for more details. Usually, this is caused by the PHP script memory limit being too low—if that is the case, increase it by setting the `memory_limit` parameter to a higher value and restarting the webserver. Another possible cause is a broken `conf/zabbix.conf.php` file—verify that it does not have any weird characters, especially at the end of the file.
- **Q:** Complex views, such as screens with many elements, sometimes fail to load. What could be causing this?
- **A:** Like the previous problem, check that the PHP memory limit has not been exceeded. Additionally, check the PHP script timeout (`max_execution_time` parameter) and increase it if necessary.
- **Q:** My graphs have gaps.
- **A:** It's not only graphs—data is missing in the database as well. This problem should be resolved by finding out what causes the data loss. Common reasons for this are:
  - **Network problems:** If the network is unreliable, data will be missing.
  - **An overloaded monitored device:** For example, if you have added a switch with many ports and are monitoring several items on each port very often, try increasing the intervals and disabling unneeded items.
  - **An overloaded Zabbix server:** It's usually the database. Check the system load on the Zabbix database server, especially `iowait`.
- **Q:** I had Zabbix installed and running, but it is suddenly showing me the installation screen again.
- **A:** Check the accessibility of the `conf/zabbix.conf.php` file.
- **Q:** The `conf/zabbix.conf.php` file is there, but I still see the installation screen.
- **A:** In some distribution packages, the frontend might expect the frontend configuration file to be in `/etc/zabbix/web` or a similar location. Check the package documentation.
- **Q:** I am trying to open a large page with many elements, but refresh kicks in before the page even finishes loading. How can I solve this?
- **A:** Increase the refresh period in your user profile. While the page loading speed won't be improved by that, at least the page will get a chance to load completely.
- **Q:** The clock on my server is correct, but the frontend shows incorrect times.
- **A:** Check that the time zone is set correctly in the PHP configuration.
- **Q:** Zabbix server is running, but the frontend claims it is not.
- **A:** This could be caused by multiple factors:
  - Check the `conf/zabbix.conf.php` file—the frontend uses the server address and port specified there to query the Zabbix server process.
  - Make sure no firewall is blocking connections from the frontend to the Zabbix server.
  - Make sure SELinux is not blocking connections from the frontend to the Zabbix server.
  - Make sure you have at least one trapper process enabled—they accept frontend connections. It is also possible that there are not enough trappers to service all requests. This is especially likely if the message about the server being unavailable appears only every now and then. Monitor the busy rate of the trapper processes like we did in Chapter 22, Zabbix Maintenance.
- **Q:** I am having a problem with the frontend that is not listed here.
- **A:** Check the Apache error log and PHP log—these often offer an insight into the cause. Also go to **Administration | Users or User groups** and add your user to the **Enabled debug mode** group. Afterwards, all frontend pages will have a small **Debug** control in the lower-right corner. Clicking on it will show a lot of detail about that specific page, including the exact API and SQL queries that were performed. Debug mode can use more resources—if some frontend pages stop working after enabling debug mode, try disabling it.

## Locked out of the frontend

A common mistake, performed by both new and seasoned users, is locking oneself out of the frontend. This can happen in several ways, but we're more interested here in how to get back in.

- Q: I forgot my password and tried to log in until the Zabbix frontend stopped responding.
- A: By default, Zabbix denies access for 30 seconds after five failed login attempts, so just wait for 30 seconds. You can customize these values in includes/defines.inc.php:
  - ZBX_LOGIN_ATTEMPTS: The number of unsuccessful attempts after which Zabbix denies access
  - ZBX_LOGIN_BLOCK: How long to deny access for, in seconds
- Q: I have forgotten my Admin user password, or I have been tasked with managing a Zabbix installation where the Admin user's password is not known.
- A: You can easily reset the Admin user password by directly modifying the database:
```
mysql> update zabbix.users set passwd=MD5('somepassword') where alias='Admin';
```
> Of course, replace somepassword with some other string. Keep in mind that MySQL by default saves console commands in the ~/.mysql_history file, so you might want to set the password to some temporary version and update it in the frontend later.
- Q: I changed the authentication method, but it didn't work as planned and now I can't log in anymore.
- A: You can restore Zabbix's internal authentication method by editing the database:
```
mysql> update zabbix.config set authentication_type='0' where configid='1';
```
Authentication type 0 is the internal one. For the record, other types are 1 (LDAP) and 2 (HTTP). Zabbix expects only one config table entry with a configid value of 1.

## Monitoring

Sometimes monitoring something proceeds without a hitch; sometimes it just won't work.
