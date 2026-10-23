```
2023-03-17 13:25:25.37224+00
(1 row)

forumdb=* > select now();
        now
-----------------------------
2023-03-17 13:25:25.37224+00
(1 row)

forumdb=* > commit;
COMMIT

forumdb=> begin ;
BEGIN

forumdb=* > select now();
        now
-----------------------------
2023-03-17 13:27:02.012632+00
(1 row)

forumdb=* > commit ;
COMMIT
```

Note: In PostgreSQL 16, when psql shows us a prompt like *>, it means that we are inside a transaction block.

2. Now, let’s look at an immutable function – for example, the lower(string_expression) function. The lower function accepts a string and converts it into a lowercase format. As we can see, if the input parameters are the same, the lower function always returns the same result, even if it is performed in different transactions:

```
forumdb=> begin;
BEGIN

forumdb=* > select now();
```