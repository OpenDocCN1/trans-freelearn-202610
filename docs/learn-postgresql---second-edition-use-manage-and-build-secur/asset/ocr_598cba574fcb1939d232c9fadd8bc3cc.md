> Caution: Do not write a semicolon after BEGIN – it’s not correct and it will generate a syntax error.

5. Between the BEGIN statement and the END statement, we can put our own code:

```
END;
```

6. The END instruction indicates that our code has ended:

```
$BODY$
```

7. This label closes the first label and at last, the language statement specifies PostgreSQL, in which the function is written:

```
language 'plpgsql';
```

# Dropping functions

To drop a function, we have to execute the DROP FUNCTION command followed by the name of the function and its parameters. For example, to drop the my_sum function, we have to execute:

```
forumdb=> DROP FUNCTION my_sum(integer,integer);
DROP FUNCTION
```

# Declaring function parameters

After learning about how to write a simple PL/pgSQL function, let’s go into a little more detail about the single aspects seen in the preceding section. Let’s start with the declaration of the parameters. In the next two examples, we’ll see how to define, in two different ways, the my_sum function that we have seen before.

The first example is as follows:

```
forumdb=> CREATE OR REPLACE FUNCTION my_sum(integer, integer) RETURNS integer AS
$BODY$
DECLARE
  x alias for $1;
  y alias for $2;
```