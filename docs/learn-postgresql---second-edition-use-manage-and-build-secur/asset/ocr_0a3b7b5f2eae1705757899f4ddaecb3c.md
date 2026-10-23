```
pk |   tag       | parent
----+-------------+-------
  2 | alpine linux |     1
(1 row)

forumdb=> select * from new_b_tags ;
pk |   tag   | parent
----+---------+-------
  3 | bsd unix |     1
(1 row)
```

As can be seen, the function works.

# The TG_OP variable

As shown in the official documentation at https://www.PostgreSQL.org/docs/current/plpgsql-trigger.html, control of the triggers in PostgreSQL is allowed using special variables, two of which we have already seen (the NEW variable and the OLD variable). There is another special variable called TG_OP, which tells us from which event the trigger is fired. The possible values of the TG_OP variable are INSERT, DELETE, UPDATE, and TRUNCATE.

# Triggers on UPDATE / DELETE

Now, let’s go back to the example we used in Figure 8.1. The goal we want to achieve is to create a single function that is able to handle the INSERT, DELETE, and UPDATE events, First, let’s return to the initial conditions in our environment:

```
forumdb=> truncate new_tags;
TRUNCATE TABLE
forumdb=> truncate new_a_tags;
TRUNCATE TABLE
forumdb=> truncate new_b_tags;
TRUNCATE TABLE
forumdb=> drop trigger t3_tags on new_tags cascade;
DROP TRIGGER
```

Now, as before, we will proceed step by step. The first step is to write the section of code that will be performed during the INSERT event. Then, we will see how to extend the function to manage the DELETE and UPDATE events.