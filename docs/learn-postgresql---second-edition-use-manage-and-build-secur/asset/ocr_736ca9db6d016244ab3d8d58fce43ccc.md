```
```text
psql (15.2 (Debian 15.2-1.pgdg110+1))
Type "help" for help.

forumdb=#
```
```

And then let’s execute:

```
```sql
forumdb=# CREATE EVENT TRIGGER tr_avoid_alter_table ON ddl_command_end
EXECUTE FUNCTION forum.f_avoid_alter_table();
CREATE EVENT TRIGGER
```
```

Remember that we have connected as a postgres user to the database forumdb, so we have to specify the schema in which the postgres user can find the f_avoid_alter_table() function.

At this point, the trigger is active, and the function will be fired for every DDL command once the system approaches the end of a command.

It is now possible to test the trigger and see whether a user is allowed to execute ALTER TABLE:

```
```text
forumdb=> ALTER TABLE tags ADD COLUMN thumbs_up int DEFAULT 0;
ERROR:  Cannot execute an ALTER TABLE!
CONTEXT:  PL/pgSQL function f_avoid_alter_table() line 9 at RAISE
```
```

As we can see, an exception is raised as soon as the ALTER TABLE command is executed, and we have this behavior for not only the non-superuser (as we’ve just seen) but also the superuser; this is because the event trigger we wrote intercepts the alter table command and modifies its behavior:

```
```text
forumdb=# ALTER TABLE forum.tags ADD COLUMN thumbs_up int DEFAULT 0;
ERROR:  Cannot execute an ALTER TABLE!
CONTEXT:  PL/pgSQL function forum.f_avoid_alter_table() line 9 at RAISE
```
```

While event triggers can be used, as in the preceding example, to prevent users from executing particular commands, a better strategy is to avoid inappropriate command executions by means of permissions whenever possible. Event triggers are complex and are used to provide support for things such as logical replication, auditing, and other infrastructures.

# Summary

In this chapter, we covered the topic of triggers and rules. We explored rules and triggers using some identical examples. We established that rules are simple event handlers and triggers are complex event handlers.