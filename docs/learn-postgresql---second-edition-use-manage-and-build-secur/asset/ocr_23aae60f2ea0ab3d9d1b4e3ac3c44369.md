+   - Loop statements
+   - Exception handler statements

# Conditional statements

The PL/pgSQL language can manage IF-type conditional statements and CASE-type conditional statements.

# IF statements

In PL/pgSQL, the syntax of an IF statement is as follows:

```
IF boolean-expression THEN
  statements
[ ELSIF boolean-expression THEN
  statements
[ ELSIF boolean-expression THEN
  statements
  ...
]
]
[ ELSE
  statements ]
END IF;
```

For example, say we want to write a function that, when given the two input values, x and y, returns the following:

+   - *first parameter is greater than second parameter* if x > y
+   - *second parameter is greater than first parameter* if x < y
+   - *the 2 parameters are equals* if x = y

We have to write the following function:

```
forumdb=> CREATE OR REPLACE FUNCTION my_check(x integer default 0, y integer default 0) RETURNS text AS
$BODY$
BEGIN
  IF x > y THEN
  return 'first parameter is greater than second parameter';
```