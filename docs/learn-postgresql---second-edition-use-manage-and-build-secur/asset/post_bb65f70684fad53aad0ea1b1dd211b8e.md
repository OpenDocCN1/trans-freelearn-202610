
```
where NEW.tag ilike 'b%'
DO INSTEAD insert into new_b_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
```

- 7. For now, let’s use the same procedure we used in the rules by creating a new function, which will then be fired from the trigger:

```
forumdb=> CREATE OR REPLACE FUNCTION f2_tags() RETURNS trigger as $$
BEGIN
    IF lower(substring(NEW.tag from 1 for 1)) = 'b' THEN
    insert into new_b_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    RETURN NULL;
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE 'plpgsql';
CREATE FUNCTION

forumdb=> CREATE TRIGGER t2_tags BEFORE INSERT on new_tags FOR EACH ROW EXECUTE PROCEDURE f2_tags();
CREATE TRIGGER
```

- 8. The lower statement, `(substring(NEW.tag from 1 for 1)) = 'b'`, is practically identical to what we first saw in relation to rules. The difference is the `RETURN NULL`, which means that if the `NEW.tag` value starts with `b`, then a `NULL` value is returned to the default action and the `INSERT` on the `new_tags` table will not insert any value. If, instead, the `IF` condition is not satisfied, then the function returns `NEW` and the record is inserted into the `new_tags` table.

Let’s see if it works:

```
forumdb=> truncate new_tags;
TRUNCATE TABLE
forumdb=> truncate new_a_tags;
TRUNCATE TABLE
forumdb=> truncate new_b_tags;
TRUNCATE TABLE

forumdb=> insert into new_tags (pk,tag,parent) values (1,'bsd unix',NULL);
INSERT 0 0
```

As we can see, the `IF` condition works, and the result, `INSERT 0 0`, means that no record has been inserted into the `new_tags` table. This happened because the trigger works on the `BEFORE INSERT` event and the `IF` condition moved the record to the `new_b_tags` table.

- 9. We will now look at how to write the whole procedure using a single trigger. First, let’s go back to the initial conditions of our environment. As before, we delete the data in the tables and, using the `CASCADE` option, we delete the selected trigger and all the triggers associated with it:

```
forumdb=> TRUNCATE new_tags;
TRUNCATE TABLE
forumdb=> TRUNCATE new_a_tags;
TRUNCATE TABLE
forumdb=> TRUNCATE new_b_tags;
TRUNCATE TABLE
forumdb=> DROP TRIGGER t_tags ON new_tags CASCADE;
DROP TRIGGER
forumdb=> DROP TRIGGER t2_tags ON new_tags CASCADE;
DROP TRIGGER
```

- 10. In this last step, we will combine what we have written in the functions `f_tags()` and `f2_tags()` into a single function, `f3_tags()`, which will be fired from the `t3_tags` trigger on the event `BEFORE INSERT`:

```
forumdb=> CREATE OR REPLACE FUNCTION f3_tags() RETURNS trigger as $$
BEGIN
    IF lower(substring(NEW.tag from 1 for 1)) = 'a' THEN
        insert into new_a_tags(pk,tag,parent) values (NEW.pk,NEW.tag,NEW.parent);
        RETURN NEW;
    ELSIF lower(substring(NEW.tag from 1 for 1)) = 'b' THEN
        insert into new_b_tags(pk,tag,parent) values (NEW.pk,NEW.tag,NEW.parent);
    RETURN NULL;
    ELSE
        RETURN NEW;
    END IF;
END;
$$ 
LANGUAGE 'plpgsql';
CREATE FUNCTION

forumdb=> CREATE TRIGGER t3_tags BEFORE INSERT on new_tags FOR EACH ROW EXECUTE PROCEDURE f3_tags();
CREATE TRIGGER
```

This function contains the logic of the two functions previously seen. This way, we can solve the problem more elegantly by using a single function and a single trigger. Let’s see if it works:

```
forumdb=> insert into new_tags (pk,tag,parent) values (1,'operating systems',NULL);
INSERT 0 1

forumdb=> insert into new_tags (pk,tag,parent) values (2,'alpine linux',1);
INSERT 0 1
forumdb=> insert into new_tags (pk,tag,parent) values (3,'bsd unix',1);
INSERT 0 0

forumdb=> select * from new_tags ;
pk |      tag      | parent
----+---------------+--------
  1 | operating systems |
  2 | alpine linux  |      1
(2 rows)

forumdb=> select * from new_a_tags ;
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

## The TG_OP variable

As shown in the official documentation at https://www.PostgreSQL.org/docs/current/plpgsql-trigger.html, control of the triggers in PostgreSQL is allowed using special variables, two of which we have already seen (the `NEW` variable and the `OLD` variable). There is another special variable called `TG_OP`, which tells us from which event the trigger is fired. The possible values of the `TG_OP` variable are `INSERT`, `DELETE`, `UPDATE`, and `TRUNCATE`.

## Triggers on UPDATE / DELETE

Now, let’s go back to the example we used in Figure 8.1. The goal we want to achieve is to create a single function that is able to handle the `INSERT`, `DELETE`, and `UPDATE` events. First, let’s return to the initial conditions in our environment:

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

Now, as before, we will proceed step by step. The first step is to write the section of code that will be performed during the `INSERT` event. Then, we will see how to extend the function to manage the `DELETE` and `UPDATE` events.

The function that will handle all three events will be the `fcopy_tags()` function; this function will be invoked by the `tcopy_tags` trigger. The `fcopy_tags()` function, using the `TG_OP` variable, will be able to discriminate between the `INSERT`, `UPDATE`, and `DELETE` events.

Let’s start by writing the `fcopy_tags()` function to handle the `INSERT` event:

```
forumdb=> CREATE OR REPLACE FUNCTION fcopy_tags() RETURNS trigger as $$
BEGIN
IF TG_OP = 'INSERT' THEN
    IF lower(substring(NEW.tag from 1 for 1)) = 'a' THEN
        insert into new_a_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    ELSIF lower(substring(NEW.tag from 1 for 1)) = 'b' THEN
        insert into new_b_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    END IF;
    RETURN NEW;
END IF;
END;
$$ LANGUAGE 'plpgsql';
CREATE FUNCTION

forumdb=> CREATE TRIGGER tcopy_tags_ins BEFORE INSERT on new_tags FOR EACH ROW EXECUTE PROCEDURE fcopy_tags();
CREATE TRIGGER
```

Now let’s see if, for the `INSERT` event, this code works:

```
forumdb=> insert into new_tags (pk,tag,parent) values (1,'operating systems',NULL);
INSERT 0 1
forumdb=> insert into new_tags (pk,tag,parent) values (2,'alpine linux',1);
INSERT 0 1
forumdb=> insert into new_tags (pk,tag,parent) values (3,'bsd unix',1);
INSERT 0 1

forumdb=> select * from new_a_tags ;
pk |     tag     | parent
----+--------------+-------
2  | alpine linux |     1
(1 row)

forumdb=> select * from new_b_tags ;
pk |     tag     | parent
----+--------------+-------
3  | bsd unix |     1
(1 row)

forumdb=> select * from new_tags ;
pk |          tag          | parent
----+----------------------+--------
1  | operating systems |
2  | alpine linux |     1
3  | bsd unix |     1
(3 rows)
```

It is clear that it works!

Next, let’s handle the `DELETE` event. The things we need to do are the following:

- Add some lines of code to the function to manage the `DELETE` operation.
- Add a new trigger that is able to handle the `DELETE` event.

The function becomes as follows:

```
forumdb=> CREATE OR REPLACE FUNCTION fcopy_tags() RETURNS trigger as $$
BEGIN
IF TG_OP = 'INSERT' THEN
    IF lower(substring(NEW.tag from 1 for 1)) = 'a' THEN
        insert into new_a_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    ELSIF lower(substring(NEW.tag from 1 for 1)) = 'b' THEN
        insert into new_b_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    END IF;
    RETURN NEW;
END IF;
IF TG_OP = 'DELETE' THEN
    IF lower(substring(OLD.tag from 1 for 1)) = 'a' THEN
        DELETE FROM new_a_tags WHERE pk = OLD.pk;
        ELSIF lower(substring(OLD.tag from 1 for 1)) = 'b' THEN
        DELETE FROM new_b_tags WHERE pk = OLD.pk;
    END IF;
    RETURN OLD;
END IF;
END;
$$ 
LANGUAGE 'plpgsql';
CREATE FUNCTION
```

This piece of code was added:

```
IF TG_OP = 'DELETE' THEN
    IF lower(substring(OLD.tag from 1 for 1)) = 'a' THEN
        DELETE FROM new_a_tags WHERE pk = OLD.pk;
    ELSIF lower(substring(OLD.tag from 1 for 1)) = 'b' THEN
        DELETE FROM new_b_tags WHERE pk = OLD.pk;
    END IF;
    RETURN OLD;
END IF;
```

This piece of code deletes the data in the `new_a_tags` and `new_b_tags` tables if the record to be deleted begins with the letter `a` or `b`. Now we have to create a new trigger that is able to handle `DELETE` events:

```
forumdb=> CREATE TRIGGER tcopy_tags_del
AFTER DELETE on new_tags FOR EACH ROW EXECUTE PROCEDURE fcopy_tags();
CREATE TRIGGER
```

The trigger is executed `AFTER DELETE`; in this case, it would have made no difference if we created the `TRIGGER` `BEFORE` or `AFTER INSERT` functions. Let’s see if this trigger on the `DELETE` event works:

```
forumdb=> delete from new_tags where pk=2;
DELETE 1
forumdb=> delete from new_tags where pk=3;
DELETE 1
```
