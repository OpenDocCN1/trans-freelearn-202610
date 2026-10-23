4. Now, if we want to search for all the records that have the user_on_line field set to true, we have to perform the following:

```
forumdb=> \x
Expanded display is on.
forumdb=> select * from users where user_on_line = true;
-[ RECORD 1 ]+---------------------
pk            | 1
username      | luca_ferrari
gecos         |
email         | luca@pgtraining.com
user_on_line  | t
```

5. If we want the search for all the records that have the user_on_line field set to NULL, as we saw in Chapter 4, Basic Statements, we have to perform the following:

```
forumdb=> select * from users where user_on_line is NULL;
-[ RECORD 1 ]+---------------------
pk            | 2
username      | enrico_pirozzi
gecos         |
email         | enrico@pgtraiing.com
user_on_line  |
-[ RECORD 2 ]+---------------------
pk            | 3
username      | newuser
gecos         |
email         | newuser@pgtraining.com
user_on_line  |
```

Thus, we have explored the Boolean data type.

# Numeric data type

PostgreSQL supports several types of numeric data types; the most used ones are as follows:

+   - integer or int4 (4-byte integer number).
+   - bigint or int8 (8-byte integer number).
+   - real (4-byte variable precision, inexact with 6-decimal-digit precision).