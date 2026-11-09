```
)
)
FROM message
WHERE mid=52;
```

Result:

4:51am Friday, January 21, 2000

![](bbox=[0.23, 0.3, 0.29, 0.36])

> MySQL date and time functions, such as day() and year(), are described in their documentation: http://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html, and their string functions, such as concat(), can be found here: http://dev.mysql.com/doc/refman/5.7/en/string-functions.html. Other database management systems will have similar versions of these functions.

+   - Option 2: Use the more sophisticated date_format() MySQL function. This function takes a series of string specifiers for how you want the date to be formatted. There is a very long list of these specifiers in the MySQL documentation. A completed example to convert the date into our desired format is shown in the following code:

```
SELECT date_format(date, '%l:%i%p, %W, %M %e, %Y')
FROM message
WHERE mid=52;
```

Result:

4:51AM, Friday, January 21, 2000

This is pretty close to what we said we wanted, and it is much shorter than Option 1. The only difference is that the a.m./p.m. is capitalized. If we really want it lowercased, we can do this:

```
SELECT concat(
    date_format(date, '%l:%i'),
    lower(date_format(date,'%p ')),
    date_format(date,'%W, %M %e, %Y')
)
FROM message
WHERE mid=52;
```

Result:

4:51am, Friday, January 21, 2000