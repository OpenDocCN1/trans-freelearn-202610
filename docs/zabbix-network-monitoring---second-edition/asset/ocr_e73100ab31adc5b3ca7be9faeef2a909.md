If that does not work, double-check the password, path, and permissions to the file. We use mysqladmin for this test, but both mysql and mysqladmin should use the same procedure for finding the .my.cnf file and reading the password from it. Now that we know it's working, let's turn to zabbix_get again (no agent restart is needed as we did not modify the agent configuration file this time):

```
$ zabbix_get -s 127.0.0.1 -k mysql.queries[select]
```

But the result seems weird:

```
ERROR 1045 (28000): Access denied for user 'zabbix'@'localhost' (using password: NO)
```

> In some cases, when using systemd, the home directory might be set—if so, skip the next change, but keep in mind this potential pitfall.

It's failing still. And with the same error message. If we carefully read the full error, we'll see that the password is still not used. How could that be?

> It does not matter which user account we run zabbix_get as—it connects to the running agent daemon over a TCP port, thus when the user parameter command is run, information about the user running zabbix_get has no impact at all.

The environment is not initialized for user parameter commands. This includes several common variables, and one we are quite interested in—HOME. This variable is used by the MySQL client to determine where to look for the .my.cnf file. If the variable is missing, this file (and in turn, the password) can't be found. Does that mean we're doomed? Of course not, we wouldn't let such a minor problem stop us. We simply have to tell MySQL where to look for this file, and we can use a very simple method to do that. Edit zabbix_agentd.conf again and change our user parameter line to read as follows:

```
UserParameter=mysql.queries[*],HOME=/home/zabbix mysql -u zabbix -N -e "show global status like 'Com_$1';" | awk '{print $2}'
```

> If you installed from packages, use the directory which is set as the home directory for the zabbix user.