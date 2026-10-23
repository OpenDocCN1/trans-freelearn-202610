```
CASE x
WHEN 1 THEN return 'value = 1';
WHEN 2 THEN return 'value = 2';
ELSE return 'value >= 3 ';
END CASE;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

The preceding my_check_value function returns the following:

+   - value = 1 if x = 1
+   - value = 2 if x = 2
+   - value >= 3 if x >= 3

We can see this to be true here:

```
forumdb=> select my_check_value(1);
my_check_value
----------------
value = 1
(1 row)

forumdb=> select my_check_value(2);
my_check_value
----------------
value = 2
(1 row)

forumdb=> select my_check_value(3);
my_check_value
----------------
value >= 3
(1 row)
```

Now, let’s see an example of the searched CASE syntax:

```
forumdb=> CREATE OR REPLACE FUNCTION my_check_case(x integer default 0, y integer default 0) RETURNS text AS
```