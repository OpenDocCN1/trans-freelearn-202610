In this case, the trigger must be defined with AFTER UPDATE and not with BEFORE UPDATE because in the UPDATE section, we have the instruction DELETE FROM new_tags WHERE pk = OLD.pk; if the trigger had been defined with BEFORE UPDATE, we would have had an error because we would have attempted to delete a record reserved for UPDATE.

Let’s see if the complete function works:

```
forumdb=> select * from new_tags;
 pk |      tag      | parent
----+---------------+--------
  1 | operating systems |
  2 | alpine linux  |     1
  3 | bsd unix      |     1
(3 rows)

forumdb=> select * from new_a_tags;
 pk |      tag      | parent
----+---------------+--------
  2 | alpine linux |     1
(1 row)

forumdb=> select * from new_b_tags;
 pk |      tag      | parent
----+---------------+--------
  3 | bsd unix |     1
(1 row)

forumdb=> update new_tags set tag='apple dos' where pk=3;
UPDATE 1

forumdb=> select * from new_a_tags;
 pk |      tag      | parent
----+---------------+--------
```