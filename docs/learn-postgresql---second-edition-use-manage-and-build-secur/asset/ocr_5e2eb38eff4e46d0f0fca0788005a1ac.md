PostgreSQL handles the following NoSQL data types:

+   - hstore
+   - xml
+   - json/jsonb

We will now talk about hstore and json.

# The hstore data type

hstore was the first NoSQL data type that was implemented in PostgreSQL. This data type is used for storing key-value pairs in a single value. Before working with the hstore data type, we need to enable the hstore extension on our server:

```
forumdb=> create extension hstore ;
CREATE EXTENSION
```

Let’s look at how we can use the hstore data type with an example. Suppose that we want to show all posts with their usernames and their categories:

```
forumdb=> select p.pk,p.title,u.username,c.title as category
from posts p
inner join users u on p.author=u.pk
left join categories c on p.category=c.pk
order by 1;
-[ RECORD 1 ]---------------------------------
pk      | 5
title   | Indexing PostgreSQL
username | luca_ferrari
category | Database
-[ RECORD 2 ]---------------------------------
pk      | 6
title   | Indexing Mysql
username | luca_ferrari
category | Database
-[ RECORD 3 ]---------------------------------
pk      | 7
title   | A view of  Data types in C++
username | enrico_pirozzi
category | Programming Languages
```