The function invoked by the trigger must be defined in a particular way, as shown in the prototype here:

```
CREATE OR REPLACE FUNCTION function_name RETURNS trigger as
$$
DECLARE
....
BEGIN
    RETURN
END;
$$
LANGUAGE 'plpgsql';
```

The functions that are called by the triggers are functions that have no input parameters and must return a TRIGGER type; these functions take the parameters from the NEW/OLD records. Starting with this prototype of the preceding function, a possible TRIGGER definition of the BEFORE INSERT event can be described as follows:

```
CREATE TRIGGER trigger_name BEFORE INSERT on table_name FOR EACH ROW
EXECUTE PROCEDURE function_name.
```

There is also this syntax:

```
CREATE TRIGGER trigger_name BEFORE INSERT on table_name FOR EACH STATEMENT
EXECUTE PROCEDURE function_name.
```

The difference between FOR EACH ROW and FOR EACH STATEMENT is that:

+   - A trigger defined with FOR EACH ROW is executed for each row involved in the operation (for example, for each row inserted, updated, or deleted) that satisfies the condition of the trigger.
+   - A trigger defined with FOR EACH STATEMENT is executed only once for each SQL statement that satisfies the trigger’s condition, no matter how many rows are involved in the operation.

In the next section, we will try to implement what we wrote with the rules, this time applying triggers.