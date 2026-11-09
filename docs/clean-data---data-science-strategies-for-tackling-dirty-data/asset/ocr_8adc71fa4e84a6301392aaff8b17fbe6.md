# Example two – finding the UTF-8 and Latin-1 equivalents of Unicode characters stored in MySQL

The following code will use the convert() function and the RLIKE operator to print the UTF-8 equivalent of Unicode strings that have been saved to MySQL using Latin-1. This is useful if you have multibyte data that has been stored in Latin-1 encoded text columns in MySQL, an unfortunately common occurrence as Latin-1 is still the default (as of MySQL 5).

This code uses the publicly available and widely used Movielens database of movies and their reviews. The entire movielens dataset is widely available on many websites, including from the original project site: http://grouplens.org/datasets/movielens/. Another SQL-friendly link is here: https://github.com/ankane/movielens.sql. To make it easier to work through these examples, the CREATE and INSERT statements for just a small subset of the relevant rows have been made available on the author's GitHub site for this book: https://github.com/megansquire/datacleaning/blob/master/ch2movies.sql. This way, if you prefer, you can simply create this one table using that code and work through the following example.

```
SELECT convert(
    convert(title USING BINARY) USING latin1
) AS 'latin1 version',
convert(
    convert(title USING BINARY) USING utf8
) AS 'utf8 version'
FROM movies
WHERE convert(title USING BINARY)
RLIKE concat(
    '[',
    unhex('80'),
    '-]',
    unhex('FF'),
    ']'
);
```