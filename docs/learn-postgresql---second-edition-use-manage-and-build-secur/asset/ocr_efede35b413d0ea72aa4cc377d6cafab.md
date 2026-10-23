The first step is to define a function that will be executed once the trigger has been fired; such a function needs to inspect the DDL statement properties to understand whether it has been invoked by means of an ALTER TABLE command. The introspection is done using the `pg_event_trigger_ddl_commands()` special function, which returns a tuple for every DDL statement executed within the same command. Such tuples contain a field named `command_tag`, which reports the command group (uppercase), and `object_type`, which reports the object type (lowercase) that the DDL statement has been executed against. The function must return a trigger type, specifically an event trigger type; therefore, the function can be defined as follows:

```
```sql
forumdb=> CREATE OR REPLACE FUNCTION
f_avoid_alter_table()
RETURNS EVENT_TRIGGER
AS
$code$
DECLARE
event_tuple record;
BEGIN
    FOR event_tuple IN SELECT * FROM             pg_event_trigger_ddl_
commands() LOOP
        IF event_tuple.command_tag = 'ALTER TABLE' AND event_tuple.object_
type = 'table' THEN
            RAISE EXCEPTION 'Cannot execute an ALTER TABLE!';
        END IF;
    END LOOP;
END
$code$
LANGUAGE plpgsql;
CREATE FUNCTION
```
```

As you can see, if the function discovers that the executed command has an ALTER TABLE tag and a table object type, it raises an exception, causing the whole statement to fail.

Once the function is in place, it is possible to attach it to an event trigger, but because event triggers handle DDL statements, only superusers can create an event trigger; so first, let’s connect to the forum database as a superuser:

```
```bash
forumdb=> \q
postgres@learn_postgresql:~$ psql forumdb
```
```