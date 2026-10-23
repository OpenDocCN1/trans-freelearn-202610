# Technical requirements

Before starting, remember to start the Docker container named chapter_08, as shown below:

```
$ bash run-pg-docker.sh chapter_08
postgres@learn_postgresql:~$ psql -U forum forumdb
```

# Exploring rules in PostgreSQL

As mentioned earlier, rules are simple event handlers. At the user level, it is possible to manage all the events that perform write operations, which are as follows:

+   - INSERT
+   - DELETE
+   - UPDATE

The fundamental concept behind rules is to modify the flow of an event. If we are given an event, what we can do when certain conditions occur is as follows:

+   - Do nothing and then undo the action of that event.
+   - Trigger another event instead of the default one.
+   - Trigger another event in conjunction with the default.

So, given a write operation, for example, an INSERT operation, we can perform one of these three actions:

+   - Cancel the operation.
+   - Perform another operation instead of the INSERT.
+   - Execute the INSERT and, in the same transaction, perform another operation.

# Understanding the OLD and NEW variables

Before we start working with rules and then with triggers, we need to understand the concept of the OLD and NEW variables.

The OLD and NEW variables represent the state of the row in the table before or after the event. OLD and NEW values are cursors that represent the whole record. To better understand this, consider an UPDATE operation; in this case, the OLD variable contains the value of the record already present in the table, while the NEW variable contains the value that the record of the table will have after the UPDATE operation.