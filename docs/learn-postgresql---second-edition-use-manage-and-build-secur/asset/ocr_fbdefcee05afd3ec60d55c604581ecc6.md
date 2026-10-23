# Explaining MVCC

xmin is only a part of the story of managing MVCC. PostgreSQL labels every tuple in the database with four different fields, named xmin (already described), xmax, cmin, and cmax. Similar to what you learned about xmin, in order to make those fields appear in a query result, you need to explicitly reference them – for instance:

```
forumdb=> SELECT xmin, xmax, cmin, cmax, * FROM tags ORDER BY tag;
 xmin | xmax | cmin | cmax | pk | tag | parent
-------+------+------+------+------+------+--------
 4854 | 0 | 0 | 0 | 24 | c++ |
 4853 | 0 | 0 | 0 | 23 | java |
 4852 | 0 | 0 | 0 | 22 | perl |
 4855 | 0 | 0 | 0 | 25 | unix |
(4 rows)
```

The meaning of xmin has been already described in a previous section: it indicates the transaction identifier of the transaction that created the tuple. The xmax field, on the other hand, indicates the xid of the transaction that invalidated the tuple, for example, because it has deleted the data. The cmin and cmax fields indicate respectively the command identifiers that created and invalidated the tuple within the same transaction (PostgreSQL numbers every statement within a transaction, starting from 0).

Why is it important to keep track of the statement identifier (cmin, cmax)? Since the lowest isolation level that PostgreSQL applies is READ COMMITTED, every single statement (that is, command) in a transaction must see the snapshot of the data consolidated when the command is started.

You can see the usage of cmin and cmax within the same transaction in the following example. First of all, we begin an explicit transaction, and then we insert a couple of tuples with two different INSERT statements; this means that the created tuples will have a different cmin:

```
forumdb=> BEGIN;
BEGIN

forumdb=> SELECT xmin, xmax, cmin, cmax, tag, txid_current()
    FROM tags ORDER BY tag;
 xmin | xmax | cmin | cmax | tag | txid_current
```