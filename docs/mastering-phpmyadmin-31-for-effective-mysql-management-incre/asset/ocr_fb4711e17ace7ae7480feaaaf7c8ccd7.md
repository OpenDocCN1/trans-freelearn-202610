These sets of directives may be in two different Apache configuration files, and hence, difficult to notice. The recommended way is to use AddType. Using this, we just need to put comments on the other lines (as shown in the following snippet) and restart Apache.

```
#SetOutputFilter PHP
#SetInputFilter PHP
```

# Authentication window displayed more than once

This problem occurs when we try to start phpMyAdmin with a URL other than the one set in $cfg['PmaAbsoluteUri']. For example, a server may have more than one name, or we may try to use the IP address instead of the name.

# Column size changed by phpMyAdmin

For a more efficient column definition, MySQL itself sometimes decides to change the column type and size. This happens mostly with CHAR and VARCHAR.

# Seeing many databases that are not ours

This problem occurs mostly after an upgrade to MySQL 4. The automatic server upgrade procedure gives the global privileges CREATE TEMPORARY TABLES, SHOW DATABASES, and LOCK TABLES to all users. These privileges also enable users to see the names of all the databases (but not their tables) until we upgrade the GRANT tables privilege as described in the MySQL manual. If the users do not need these privileges, we can revoke the privileges. The users will then see only those databases to which they have access rights.

# Not being able to store a value greater than 127

This is normal if we have defined a column of type TINYINT, as 127 is the maximum value for this column type. Similar problems may arise with other numeric column types. Changing the type to INT expands the available range of values.

# Seeking support

The starting point for support is the homepage, http://www.phpmyadmin.net, which has sections on documentation and support. There, you will find links to the discussion forums and to various trackers, such as:

+   - Bug tracker
+   - Feature requests tracker