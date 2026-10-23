
```
psql (15.2 (Debian 15.2-1.pgdg110+1))
Type "help" for help.

forumdb=#
```

And then let’s execute:

```sql
forumdb=# CREATE EVENT TRIGGER tr_avoid_alter_table ON ddl_command_end
EXECUTE FUNCTION forum.f_avoid_alter_table();
CREATE EVENT TRIGGER
```

Remember that we have connected as a postgres user to the database forumdb, so we have to specify the schema in which the postgres user can find the f_avoid_alter_table() function.

At this point, the trigger is active, and the function will be fired for every DDL command once the system approaches the end of a command.

It is now possible to test the trigger and see whether a user is allowed to execute ALTER TABLE:

```text
forumdb=> ALTER TABLE tags ADD COLUMN thumbs_up int DEFAULT 0;
ERROR:  Cannot execute an ALTER TABLE!
CONTEXT:  PL/pgSQL function f_avoid_alter_table() line 9 at RAISE
```

As we can see, an exception is raised as soon as the ALTER TABLE command is executed, and we have this behavior for not only the non-superuser (as we’ve just seen) but also the superuser; this is because the event trigger we wrote intercepts the alter table command and modifies its behavior:

```text
forumdb=# ALTER TABLE forum.tags ADD COLUMN thumbs_up int DEFAULT 0;
ERROR:  Cannot execute an ALTER TABLE!
CONTEXT:  PL/pgSQL function forum.f_avoid_alter_table() line 9 at RAISE
```

While event triggers can be used, as in the preceding example, to prevent users from executing particular commands, a better strategy is to avoid inappropriate command executions by means of permissions whenever possible. Event triggers are complex and are used to provide support for things such as logical replication, auditing, and other infrastructures.

## Summary

In this chapter, we covered the topic of triggers and rules. We explored rules and triggers using some identical examples. We established that rules are simple event handlers and triggers are complex event handlers.

We introduced the concept of trigger variables:

- NEW
- OLD
- TG_OP

As well as data manipulation-based triggers, we briefly introduced the PostgreSQL event triggers that allow developers and database administrators to have more control over firing and executing functions.

We have come to understand that triggers are extremely complex event handlers. In this chapter, we started to show the power of the tools made available to the PostgreSQL DBA; in the next chapter, we will talk about partitioning, and we will utilize the topics covered in this chapter to do so.

## Verify your knowledge

- What is the NEW record?

The NEW record is the record that is going to be processed before an INSERT statement or an UPDATE statement, for example:

```
insert into mytable(id,city_name) values (1,'New York')
```

```
NEW.id = 1
NEW.city_name = 'New York'
```

See the section *Exploring rules in PostgreSQL* for more details.

- Can we execute an INSERT on two tables in a single transaction using rules?

Yes, we can; we can make it using the ALSO clause. See the section *Exploring rules in PostgreSQL* for more details.

- Can we make all the things we do with rules using triggers?

Yes, we can; by using triggers, we can make all the things we do with rules and more. See the section *Managing triggers in PostgreSQL* for more details.

- Can we know if a trigger has been fired from an INSERT event, from an update EVENT, or from a DELETE event?

Yes, we can, using the TG_OP variable. See the section *Managing triggers in PostgreSQL* for more details.

- Can we write an audit procedure that informs us when a DDL has been executed?

Yes, we can, using event triggers. See the section *Event triggers* for more details.

## References

- PostgreSQL rules on the INSERT, UPDATE, and DELETE official documentation: https://www.PostgreSQL.org/docs/current/rules-update.html
- PostgreSQL trigger functions official documentation: https://www.PostgreSQL.org/docs/current/plpgsql-trigger.html
- PostgreSQL ALTER TRIGGER official documentation: https://www.PostgreSQL.org/docs/current/sql-altertrigger.html
- PostgreSQL DROP TRIGGER official documentation: https://www.PostgreSQL.org/docs/current/sql-droptrigger.html
- PostgreSQL EVENT TRIGGER official documentation: https://www.postgresql.org/docs/current/functions-event-triggers.html

## Learn more on Discord

To join the Discord community for this book – where you can share feedback, ask questions to the author, and learn about new releases – follow the QR code below:

https://discord.gg/jYWCjF6Tku

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_301_0.png)

# 9

# Partitioning

In the previous chapter, we talked about rules and triggers. In this chapter, we will talk about partitioning. Partitioning is a technique that allows us to split a huge table into smaller tables to make queries more efficient. In this chapter, we will see how we can partition data, and, in some cases, how we can use the rules and triggers seen in the previous chapter to make partitioning possible. We will start by introducing the basic concepts of partitioning, and then we will see the possibilities PostgreSQL offers to implement partitioning.

This chapter will cover the following topics:

- Basic concepts
- Partitioning using table inheritance
- Declarative partitioning

## Technical requirements

The chapter examples can be run on the chapter_09 Docker image that you can find in the book’s GitHub repository: https://github.com/PacktPublishing/Learn-PostgreSQL-Second-Edition. For installation and usage instructions for the Docker images for this book, please refer to Chapter 1, Introduction to PostgreSQL.

## Basic concepts

First of all, let’s try to understand why we have to partition data. We should start by saying that a common constant of all databases is that their size always grows. It is, therefore, possible that a database, after a few months of growth, can reach a size of gigabytes, terabytes, or even petabytes.

Another thing we must always keep in mind is that not all tables grow at the same rate or to the same level; some tables are bigger than others and some indexes too are bigger than other indexes.

We also need to know that there is a part of our server’s RAM, shared among all the PostgreSQL processes, that is used to manage the data present in tables. This part of the server’s RAM is called `shared_buffers`.

The way PostgreSQL works is as follows:

1. Data is taken from hard disks.
2. Data is placed in shared buffers.
3. Data is processed in shared buffers.
4. Data is downloaded to disks.

Typically, in a dedicated server only for PostgreSQL, the size of `shared_buffers` is about one-third or one-quarter of the total server RAM. A useful link to set some PostgreSQL configuration parameters (including a recommended size for `shared_buffers`) is https://pg tune.leopard.in.ua.

When a table grows excessively compared to the `shared_buffers` size, there is a possibility that performance will decrease. In this case, partitioning data can help us. Partitioning data means splitting a very large table into smaller tables in a way that is transparent to the client program. The client program will think that the server still has only a single table, but having smaller tables also means having smaller indexes that have higher chances of staying in memory, which in turn increases data performance; moreover, having smaller tables means that the vacuum processes works on smaller tables, which minimizes the execution time of the vacuum processes. Finally, when running a vacuum full, the disk space used by the table is doubled, therefore having many small tables instead of one large one significantly reduces any impact from this issue. Data partitioning can be done in two ways:

- Using table inheritance (the only possible way for PostgreSQL < 10)
- Using declarative partitioning (the best way starting from version 10)

After figuring out when it is recommended to partition data, let’s see what types of table partitioning are possible. PostgreSQL supports three types of declarative partitioning:

- Range partitioning
- List partitioning
- Hash partitioning

We will now describe these three methods in detail.
