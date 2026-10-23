And let’s query what the data in the table is:

```
```text
forumdb=> SELECT xmin, * FROM tags;
 xmin | pk | tag  | parent
------+----+------+--------
 4824 |  9 | linux |
 4825 | 10 | BSD   |
 4826 | 11 | Java  |
 4827 | 12 | Perl  |
 4828 | 13 | Raku  |
(5 rows)
```
```

As you can see, the xmin field has a different (self-incremented) value for every single tuple inserted, which means a new transaction identifier (xid) has been assigned to the tuple or, more precisely, to the statement that executed INSERT. This means that every single statement is executed in its own single-statement wrapping transaction.

> The fact that you see instances of xid incremented by a single unit is because, on the machine used for the examples, there is no concurrency, that is, no other database activity is going on. However, you cannot make any predictions about what the next xid will be in a live system, with different concurrent connections and running statements.

What if we had inserted all the preceding tags in one shot, being sure that if only one of them could not be stored for any reason, all of them would disappear? To this aim, we could use *explicit transactions*. An explicit transaction is a group of statements with a well-established transaction boundary: you issue a BEGIN statement to mark the start of the transaction, and either COMMIT or ROLLBACK to end the transaction. If you issue COMMIT, the transaction is marked as successful; therefore, the modified data is stored permanently. On the other hand, if you issue ROLLBACK, the transaction is considered to have failed and all changes disappear.

Let’s see this in practice – add another bunch of tags, but this time within a single explicit transaction:

```
```text
forumdb=> BEGIN;
BEGIN
forumdb=*> INSERT INTO tags( tag ) VALUES( 'PHP' );
INSERT 0 1
```
```