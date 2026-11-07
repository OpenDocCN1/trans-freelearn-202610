# Available character sets and collations

The **Charsets** menu tab on the homepage opens the Server view for the **Charsets** subpage, which lists the character sets and collations supported by the MySQL server. The default collation for each character set is shown with a different background color (using the row-marking color defined in `$cfg['BrowseMarkerColor']`).

![](bbox=[0.18, 0.27, 0.82, 0.59])

# The binary log

If MySQL's binary log is active on our server, the menu in the Server view changes so that a **Binary log** tab appears. This tab gives access to an interface through the `SHOW BINLOG EVENTS` command. This command produces the list of SQL statements that have updated data on our servers. This list could be huge, and currently phpMyAdmin does not limit its display with a pagination technique. Hence, we could hit the browser's memory limits, which depend on the particular browser we are using.