This query makes a Cartesian product between the category table and the posts table. It can also be called a cross join:

![](bbox=[0.26, 0.14, 0.73, 0.3])

Figure 5.1: A cross join

The same query can also be written in the following way:

```
forumdb=>  select c.pk,c.title,p.pk,p.category,p.title from categories c CROSS JOIN posts p;
 pk |      title      | pk | category |      title
----+---------------+----+----------+---------------
 1 | Database       | 1 | 1 | Indexing PostgreSQL
 2 | Unix           | 1 | 1 | Indexing PostgreSQL
 3 | Programming Languages | 1 | 1 | Indexing PostgreSQL
 4 | New Category   | 1 | 1 | Indexing PostgreSQL
 5 | Database       | 1 | 1 | Indexing PostgreSQL
 1 | Database       | 2 | 1 | Indexing Mysql
 2 | Unix           | 2 | 1 | Indexing Mysql
 3 | Programming Languages | 2 | 1 | Indexing Mysql
 4 | New Category   | 2 | 1 | Indexing Mysql
 5 | Database       | 2 | 1 | Indexing Mysql
 1 | Database       | 3 | 3 | Data types in C++
 2 | Unix           | 3 | 3 | Data types in C++
 3 | Programming Languages | 3 | 3 | Data types in C++
 4 | New Category   | 3 | 3 | Data types in C++
 5 | Database       | 3 | 3 | Data types in C++
(15 rows)
```