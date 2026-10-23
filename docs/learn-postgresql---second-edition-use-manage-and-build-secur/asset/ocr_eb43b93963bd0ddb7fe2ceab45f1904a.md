# Subqueries and the IN/NOT IN condition

Let's start with the IN operator; we can use the IN operator inside a where clause instead of using multiple OR conditions. For example, if you wanted to search for all categories that have the value pk=1 or the value pk=2, you would have to perform the following statement:

```
forumdb=> select * from categories where pk=1 or pk=2;
 pk | title | description
-----+-------+-----------------------------
 1 | Database | Database related discussions
 2 | Unix | Unix and Linux discussions
(2 rows)
```

Another way to reach the same outcome is the following:

```
forumdb=> select * from categories where pk in (1,2);
 pk | title | description
-----+-------+-----------------------------
 1 | Database | Database related discussions
 2 | Unix | Unix and Linux discussions
(2 rows)
```

An operator similar to the IN operator but with reverse functionality is the NOT IN operator. For example, if we wanted to search for all categories that do not have pk=1 or pk=2, we would have to execute the following:

```
forumdb=> select * from categories where pk not in (1,2);
 pk | title | description
----+--------------------------------+-----------------------------
 3 | Programming Languages | All about programming languages
 4 | New Category | (NULL)
 5 | Database | PostgreSQL
(3 rows)
```

Now, we can insert some data into users and the posts table:

```
forumdb=> insert into users (username,email) values ('luca_ferrari','luca@pgtraining.com'),('enrico_pirozzi','enrico@pgtraining.com');
INSERT 0 2
```