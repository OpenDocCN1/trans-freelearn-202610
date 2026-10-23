```
```text
3 | PROGRAMMING LANGUAGES | All about programming languages
(1 row)
```
```

That will reveal, with verbose logging, the following:

```
```text
LOG:  duration: 19.011 ms  statement: PREPARE my_query( text ) AS SELECT *
FROM forum.categories WHERE title like $1;
LOG:  duration: 6.539 ms  statement: EXECUTE my_query( 'PROGRAMMING%' );
```
```

As you can see, in the logs, there is everything you need to reconstruct what the user has done, but that is not so simple. You have to understand that the two lines are related to each other and that the session from which the statements have been executed is the same. This is not always possible—especially if other queries are logged between the two lines you are interested in.

Moreover, it could happen that the logs do not report all the information you need—perhaps because you chose to not log statements that execute faster than a threshold.

Therefore, while you can use logging to perform auditing, that is not always the best choice. In this section, you will learn about the PgAudit extension, which was created to provide a reliable and easy-to-use auditing infrastructure. PgAudit exploits the excellent PostgreSQL logging facility; therefore, you need to configure your logging infrastructure in an appropriate way, as you will see in the next subsections.

Before we dig into the configuration and usage of PgAudit, there are some details and concepts that have to be explained. PgAudit can work in two different ways: auditing by session or by object. The former is a quick and simple way to audit a part of (or a whole) session by a user or an application; the latter is a more complex and fine-grained way of logging actions related to specific database objects (for example, who deleted rows from that table?).

Auditing by session works by simply configuring the categories of statements to audit within a session. On the other hand, auditing by object requires you to configure individual database roles that, depending on their set of permissions, will trigger the auditing of specific actions. In the following subsections, you will see both ways used to audit.

# Installing PgAudit

The fastest way to install PgAudit is to use the operating system package manager. For example, on a GNU/Debian or Ubuntu system you can type:

```
```bash
$ sudo apt install pgaudit-16-pgaudit
```
```