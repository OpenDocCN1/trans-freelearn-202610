However, if in our number we don’t have the first digit, the query will work:

```
forumdb=> select 0.123456789::numeric(10,10) as my_field;
my_field
-------------
0.1234567890
(1 row)
```

Now let’s go back to the example of the previous paragraph, which provided an incorrect sum, and let’s repeat it using the numeric type:

```
forumdb=> select sum(0.1::numeric(2,2)) from generate_series(1,10);
sum
-----
1.00
(1 row)
```

As we can see, now the value of the sum is correct; so, the correct way to represent money is using a numeric data type.

Thus, we have learned all about the various numeric data types.

# Character data type

The most used character data types in PostgreSQL are the following:

+   - character(n)/char(n) (fixed-length, blank-padded)
+   - character varying(n)/varchar(n) (variable length with a limit)
+   - varchar/text (variable unlimited length)

Now, we will look at some examples to see how PostgreSQL manages these kinds of data types.

# Chars with fixed-length data types

We will check out how they work using the following example:

+   1. Let’s start by creating a new test table:

```
forumdb=> create table new_tags (
pk integer not null primary key,
tag char(10)
);
CREATE TABLE
```