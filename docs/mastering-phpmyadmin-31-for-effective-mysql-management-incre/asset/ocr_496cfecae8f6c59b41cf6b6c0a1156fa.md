We can see that the EXPLAIN command has been passed to MySQL, telling us that the possible_keys used is author_language. Thus, we know that this index will be used for this type of query. If this index had not existed, the result would have been quite different.

| id | select_type | table | type | possible_keys | key | key_len | ref | rows | Extra |
|---|---|---|---|---|---|---|---|---|---|
| 1 | SIMPLE | book | ALL | NULL | NULL | NULL | NULL | 3 | Using where |

Here, possible_keys (NULL) and the type (ALL) mean that no index would be used, and all rows would need to be examined to find the desired data. Depending on the total number of rows, this could have a serious impact on the performance. We can ascertain the exact impact by examining the query timing that phpMyAdmin displays on each result page (Query took x sec), and comparing it with or without the index. However, the difference in time can be minimal if we only have limited test data, compared to a real table in production.

# Detection of index problems

To help users maintain an optimal index strategy, phpMyAdmin tries to detect some common index problems. For example, let's access the book table and add an index on the isbn column. When we display this table's structure, we get a warning:

Indexes:
| Action | Keyname | Type | Unique | Packed | Field | Cardinality | Collation | Null | Comment |
|---|---|---|---|---|---|---|---|---|---|
|  | PRIMARY | BTREE | Yes | No | isbn | 3 | A |  |  |
|  | by_title | BTREE | No | No | title (30) | 0 | A |  |  |
|  | author_language | BTREE | No | No | author_id | 0 | A |  |  |
|  |  |  |  |  | language | 0 | A |  |  |
|  | isbn | BTREE | No | No | isbn | 0 | A |  |  |
|  | description | FULLTEXT | No | No | description | 0 |  |  |  |
| Warning | The indexes PRIMARY and isbn seem to be equal and one of them could possibly be removed. |

The intention here is to warn us about an inefficient index structure when considering the whole table. We don't need to have two indexes on the same column.