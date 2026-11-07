We can also choose which character set and collation will be used for our connection to the MySQL server using the MySQL connection collation dialog. This is passed to the MySQL server. MySQL then transforms the characters that will be sent to our browser into this character set. MySQL also interprets what it receives from the browser according to the character set information. Remember that all tables and fields have a character set information describing how their data is encoded.

![](bbox=[0.26, 0.255, 0.74, 0.285])

Normally, the default value should work. However, if we are entering some characters using a different character set, we can choose the proper character set in this dialog.

The following parameter defines both the default connection collation and character set:

```
$cfg['DefaultConnectionCollation'] = 'utf8_unicode_ci';
```

# Left panel (navigation)

The left panel contains the following elements:

+   - The logo (see the next section)
+   - The server list (if $cfg['LeftDisplayServers'] is set to TRUE)
+   - The Home link or icon (takes you back to the phpMyAdmin homepage)
+   - A Log out link or icon
+   - A link or icon leading to the Query window
+   - Icons to display phpMyAdmin and MySQL documentation
+   - The databases and tables with statistics about the number of tables per database

If $cfg['MainPageIconic'] is set to TRUE (the default), we see the icons. However, if it is set to FALSE, we see the Home, Log out, and Query window links.

The left panel can be resized by clicking and moving the vertical separation line in the preferred direction to reveal more data, in case the database or table names are too long for the default left panel size.