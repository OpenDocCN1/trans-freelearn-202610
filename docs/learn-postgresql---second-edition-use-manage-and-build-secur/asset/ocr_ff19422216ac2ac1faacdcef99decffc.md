pg_dumpall provides a particularly useful option, --globals-only, which is used to dump only intra-cluster objects, such as roles, tablespaces, and replication slots. This option is useful to dump and restore such objects across different clusters:

```
$ pg_dumpall --globals-only -f cluster.sql
```

# Parallel backups

It is possible to use parallelization to speed up backups and restores. The basic idea is to have multiple processes (and database connections), each assigned a smaller task to perform, so that performing all the tasks in parallel will provide you with better performance.

It is important to note that, often, it is not doing the backup faster that’s the problem – rather, it’s being able to perform the restoration as fast as you can. So, while it is possible to perform both backups and restoration in parallel mode, you will find restoration to be the most important one.

pg_dump allows you to specify the parallelism level via the -j command-line argument, to which you must assign a positive integer – that is, the number of parallel processes to start. pg_dump will then open parallel connections to the database in number equal to the parallelism, plus one connection to rule them all, and will force every connection to dump a separate table. This clearly means it does not make any sense to start more processes than the number of tables in your database that you need to back up.

Since all the processes will dump a single table, parallel mode is only available for the directory (-Fd) format, where every table is stored in a separate file so that processes don’t mix their writes together.

As an example, the following instruction will dump the database with three parallel jobs, thus opening four database connections:

```
$ pg_dump -Fd -f backup_forumdb -v -j 3 forumdb
...
pg_dump: finished item 3373 TABLE DATA posts
pg_dump: finished item 3370 TABLE DATA delete_posts
pg_dump: finished item 3377 TABLE DATA users
pg_dump: dumping contents of table "forum.j_posts_tags"
pg_dump: dumping contents of table "forum.new_categories"
pg_dump: dumping contents of table "forum.tags"
pg_dump: finished item 3372 TABLE DATA new_categories
```