The most trivial example is when you input a syntax error:

```
```text
forumdb=> BEGIN;
BEGIN
forumdb=*> UPDATE tags SET tag = uppr( tag );
ERROR:  function uppr(text) does not exist
LINE 1: UPDATE tags SET tag = uppr( tag );
                              ^
HINT:  No function matches the given name and argument types. You might
need to add explicit type casts.
forumdb=!> COMMIT;
ROLLBACK
```
```

When PostgreSQL encounters an error, it aborts the current transaction. Aborting a transaction means that, while the transaction is still open, it will not honor any following command nor COMMIT and will automatically issue a ROLLBACK command as soon as you close the transaction. Therefore, even if you try to work after a mistake, PostgreSQL will refuse to accept your statements:

```
```text
forumdb=> BEGIN;
BEGIN
forumdb=*> INSERT INTO tags( tag ) VALUES( 'C#' );
INSERT 0 1
forumdb=*> INSERT INTO tags( tag ) VALUES( PHP );
ERROR:  column "php" does not exist
LINE 1: INSERT INTO tags( tag ) VALUES( PHP );
forumdb=!> INSERT INTO tags( tag ) VALUES( 'Ocaml' );
ERROR:  current transaction is aborted, commands ignored until end of
transaction block
forumdb=!> COMMIT;
ROLLBACK
```
```

Anyway, handling syntax errors or misspelled object names are not the only problems you might find when running a transaction, and, anyway, those problems are quite simple to fix, but you might also find that your transaction cannot continue because there is some data constraint that prevents the statement from completing successfully. Imagine we don’t allow any tags with a description shorter than two characters:

```
```sql
forumdb=> ALTER TABLE tags
             ADD CONSTRAINT constraint_tag_length
```
```