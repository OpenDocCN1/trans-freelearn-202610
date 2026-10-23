```
ret integer;
BEGIN
  ret := x + y;
  return ret;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

The second example is as follows:

```
forumdb=> CREATE OR REPLACE FUNCTION my_sum(integer, integer) RETURNS integer AS
$BODY$
DECLARE
  ret integer;
BEGIN
  ret := $1 + $2;
  return ret;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

In the first example, we used alias; the syntax of alias is, in general, the following:

```
newname ALIAS FOR oldname;
```

In our specific case, we used the positional variable $1 as the oldname value. In the second example, we used the positional approach exactly as we did in the case of SQL functions.

# IN/OUT parameters

In the preceding example, we used the RETURNS clause in the first row of the function definition; however, there is another way to reach the same goal. In PL/pgSQL, we can define all parameters as input parameters, output parameters, or input/output parameters. For example, say we write the following:

```
forumdb=> CREATE OR REPLACE FUNCTION my_sum_3_params(IN x integer, IN y integer, OUT z integer) AS
$BODY$
```