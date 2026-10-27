```
00000000  53 51 4c 69 74 65 20 66  6f 72 6d 61 74 20 33 00  |SQLite format 3.|
00000010  10 00 02 02 00 40 20 20  00 00 00 13 00 00 00 43  |.....@ ......C|
00000020  00 00 00 00 00 00 00 00  00 00 00 50 00 00 00 04  |..........P...|
00000030  00 00 00 00 00 00 00 00  00 00 00 01 00 00 00 00  |..............|
00000040  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |..............|
00000050  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 13  |..............|
00000060  00 2e 24 80                                         |..$.|
00000064
```

Figure 4.2 – The output from the hexdump command

As you can see, the first 16 bytes contain the SQLite 3 format string. You can parse the rest of the header by using the following table as a reference:

| Offset | Size | Description |
|---|---|---|
| 0 | 16 | The header string: "SQLite format 3\000" |
| 16 | 2 | The database page size in bytes. |
| 18 | 1 | File format write version. 1 for legacy; 2 for WAL. |
| 19 | 1 | File format read version. 1 for legacy; 2 for WAL. |
| 20 | 1 | Bytes of unused "reserved" space at the end of each page. Usually 0. |
| 21 | 1 | Maximum embedded payload fraction. Must be 64. |
| 22 | 1 | Minimum embedded payload fraction. Must be 32. |
| 23 | 1 | Leaf payload fraction. Must be 32. |
| 24 | 4 | File change counter. |
| 28 | 4 | Size of the database file in pages. The "in-header database size". |
| 32 | 4 | Page number of the first freelist trunk page. |
| 36 | 4 | Total number of freelist pages. |
| 40 | 4 | The schema cookie. |
| 44 | 4 | The schema format number. Supported schema formats are 1, 2, 3, and 4. |
| 48 | 4 | Default page cache size. |
| 52 | 4 | The page number of the largest root b-tree page when in auto-vacuum or zero otherwise. |
| 56 | 4 | The database text encoding. A value of 1 means UTF-8. A value of 2 means UTF-16le. A value of 3 means UTF-16be. |
| 60 | 4 | The "user version" as read and set by the user_version pragma. |
| 64 | 4 | True (non-zero) for incremental-vacuum mode. False (zero) otherwise. |
| 68 | 4 | The "Application ID" set by PRAGMA application_id. |
| 72 | 20 | Reserved for expansion. Must be zero. |
| 92 | 4 | The version-valid-for number. |
| 96 | 4 | SQLITE_VERSION_NUMBER |

Figure 4.3 – The SQLite database header reference

To represent a table along with its data, SQLite organizes pages using a balanced tree (B-tree) data structure, which is a highly efficient way of storing data. Pages that actually contain data, also called active pages, are assigned to a node that serves the function of connecting pages to each other.