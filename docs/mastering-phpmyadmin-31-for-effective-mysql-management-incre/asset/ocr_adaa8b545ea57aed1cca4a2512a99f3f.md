# Table maintenance

During the lifetime of a table, it repeatedly gets modified; so, it continually grows and shrinks. Outages may occur on the server, leaving some tables in a damaged state.

Using the Operations subpage, we can perform various operations, which are listed next. However, not every operation is available for every table type.

+   - **Check table:** Scans all rows to verify that deleted links are correct. Also, a checksum is calculated to verify the integrity of the keys; we should get an OK message if everything is all right.
+   - **Analyze table:** Analyzes and stores the key distribution; this will be used on subsequent JOIN operations to determine the order in which the tables should be joined.
+   - **Repair table:** Repairs any corrupted data for tables in the MyISAM and ARCHIVE engines. Note that a table might be so corrupted that we cannot even go into Table view for it! In such a case, refer to the *Multi-Table Operations* section for the procedure to repair it.
+   - **Optimize table:** This is useful when the table contains overheads. After massive deletions of rows or length changes for VARCHAR fields, lost bytes remain in the table. phpMyAdmin warns us in various places (for example, in the Structure view) if it feels the table should be optimized. This operation is a kind of defragmentation for the table. In MySQL 4.x, this operation works only on tables in the MyISAM, Berkeley (DB), and InnoDB engines. In case of MySQL 5.x, the concerned tables are in the MyISAM, InnoDB, and ARCHIVE engines.
+   - **Flush table:** This must be done when there have been many connection errors and the MySQL server blocks further connections. Flushing will clear some internal caches and allow normal operations to resume.
+   - **Defragment table:** Random insertions or deletions in an InnoDB table fragment its index. The table should be periodically defragmented for faster data retrieval.

> The operations are based on the available underlying MySQL queries — phpMyAdmin only calls those queries.

# Changing table attributes

Table attributes are the various properties of a table. This section discusses the settings for some of them.