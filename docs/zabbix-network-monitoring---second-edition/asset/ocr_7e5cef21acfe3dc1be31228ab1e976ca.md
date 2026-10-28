![](bbox=[0.148, 0.09, 0.846, 0.208])

If you are using MySQL Community Edition from the packages and the version is 5.7.6 or higher, it generates a random password that is stored in logfiles. Check out the MySQL documentation at http://dev.mysql.com/doc/refman/5.7/en/linux-installation-rpm.html for more details.

Now, let's create the database. Add the user that Zabbix would connect to the database as, and grant the necessary permissions to this user:

```
```text
mysql> create database zabbix character set utf8 collate utf8_bin;
Query OK, 1 row affected (0.01 sec)
mysql> grant all privileges on zabbix.* to 'zabbix'@'localhost'
identified by 'mycreativenpassword';
Query OK, 0 rows affected (0.12 sec)
```
```

Use the password you set in the zabbix_server.conf file instead of mycreativenpassword.

Quit the MySQL client by entering the following command:

```
```text
mysql> quit
```
```

Let's populate the newly created database with a Zabbix schema and initial data. The following commands refer to the files as they appear in the Zabbix source. When installing from packages, these files could be located in a directory such as /usr/share/doc/zabbix-server-mysql-3.0.0/create/ or /usr/share/zabbix-server-mysql:

```
```text
$ mysql -u zabbix -p zabbix < database/mysql/schema.sql
$ mysql -u zabbix -p zabbix < database/mysql/images.sql
$ mysql -u zabbix -p zabbix < database/mysql/data.sql
```
```

All three importing processes should complete without any messages. If there are any errors, review the messages, fix the issue, and retry the failed operation. If the import is interrupted in the middle of the process, you might have to clear the database—the easiest way to do this is to delete the database by typing this:

```
```text
mysql> drop database zabbix;
Query OK, 0 rows affected (0.00 sec)
```
```

![](bbox=[0.183, 0.844, 0.277, 0.904])

> Be careful not to delete a database with important information! After deleting the Zabbix database, recreate it as we did before.