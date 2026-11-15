
```
SELECT COALESCE(universe, 'Standalone') AS universe,
       COUNT(*) AS title_count
FROM titles
GROUP BY COALESCE(universe, 'Standalone')
ORDER BY title_count DESC;
```

`GROUP BY` collects rows into groups. Aggregate functions like `COUNT`, `AVG`, `MIN`, and `MAX` operate on each group.

Another example: how many episodes does each series have? We can combine joining and grouping, as shown in the following code:

```
SELECT t.name,
       COUNT(e.episode_id) AS episode_count
FROM titles t
JOIN seasons s ON s.title_id = t.title_id
JOIN episodes e ON e.season_id = s.season_id
WHERE t.kind = 'series'
GROUP BY t.title_id
ORDER BY episode_count DESC;
```

The rule to remember is that when you use `GROUP BY`, every selected column must either be grouped or aggregated.

**Prompt**: Why does `NULL` behave differently from other values in comparisons and aggregates?

## Filtering groups with HAVING

`WHERE` filters rows before grouping. `HAVING` filters after grouping. Let’s see an example that uses both, as shown in the following code:

```
SELECT t.name,
       COUNT(*) AS cast_size
FROM credits c
JOIN titles t ON t.title_id = c.title_id
JOIN roles r ON r.role_id = c.role_id
WHERE r.role_name = 'actor'
GROUP BY t.title_id
HAVING COUNT(*) >= 2
ORDER BY cast_size DESC;
```

This returns only titles with two or more credited actors.

**Prompt**: Please show me some examples where `HAVING` is required and `WHERE` cannot be used.

## A useful order for designing queries

When writing a query, design it in this order:

1. Start with the table and its columns that represent what you care most about with `SELECT` and `FROM`
2. Join outward to other tables to bring in related data with `JOIN`
3. Filter rows with `WHERE`
4. Group if you need summaries with `GROUP BY`
5. Filter groups with `HAVING`
6. Sort the final result with `ORDER BY`

SQL queries read top to bottom, but they make sense when designed inside out.

## Syntax check: quick quiz

These questions catch mistakes beginners often make:

1. Which clause controls which rows are included in a result set?
   - A) `SELECT`
   - B) `WHERE`
   - C) `ORDER BY`
   - D) `GROUP BY`

2. Which clause guarantees the order of rows returned by a query?
   - A) `WHERE`
   - B) `LIMIT`
   - C) `ORDER BY`
   - D) `JOIN`

3. What does the following query return?
   ```
   SELECT COUNT(*)
   FROM titles
   WHERE kind = 'series';
   ```
   - A) A list of TV series
   - B) The number of rows in the table
   - C) The number of TV series
   - D) One row per TV series

4. Which statement is true about `GROUP BY`?
   - A) It filters rows before aggregation
   - B) It sorts the result set
   - C) Every selected column must be aggregated or grouped
   - D) It can replace `JOIN`

5. What is the difference between `WHERE` and `HAVING`?
   - A) `WHERE` filters columns, `HAVING` filters rows
   - B) `WHERE` runs after `GROUP BY`
   - C) `WHERE` filters rows before grouping, `HAVING` filters groups after grouping
   - D) They are interchangeable

6. Which `JOIN` returns only rows that have matching values on both sides?
   - A) `LEFT JOIN`
   - B) `RIGHT JOIN`
   - C) `FULL JOIN`
   - D) `INNER JOIN`

7. Why is the following query invalid?
   ```
   SELECT name, COUNT(*)
   FROM titles;
   ```
   - A) `COUNT` cannot be used with `SELECT`
   - B) `name` must be part of a `GROUP BY` clause
   - C) `titles` cannot be counted
   - D) `COUNT(*)` requires a `WHERE` clause

## Query exercises: write the SQL

For the exercises in *Table 7.2*, you should write the SQL yourself. Solutions should be runnable against the `Media` database schema used in this chapter.

| Name | Description |
| --- | --- |
| Exercise 1: simple filtering | Write a query that lists the names and release years of all films, sorted from oldest to newest. |
| Exercise 2: limiting results | Write a query that returns the three most recent titles, regardless of whether they are films or series. |
| Exercise 3: joining tables | Write a query that lists all episodes for *Star Trek: The Next Generation*, showing the season number, episode number, and episode name, in broadcast order. |
| Exercise 4: many-to-many relationships | Write a query that lists all actors who appear in *The Matrix*, along with the character they played. |
| Exercise 5: aggregation | Write a query that shows how many titles exist in each universe. Treat titles with no universe as “Standalone”. |
| Exercise 6: grouping and HAVING | Write a query that returns only titles that have two or more credited actors, along with the actor count. |
| Exercise 7: understanding joins | Write a query that lists all people in the database and the titles they are associated with. Include people even if they have only one credit. |
| Exercise 8: thinking in sets | Write a query that finds all people who appear in more than one title. |

Table 7.2: Query exercises

## A warning about SQL injection attacks

At this point, you are writing SQL by hand and running it directly against a database. That is safe because you control the input.

In real applications, SQL queries are often built using user input, such as form fields, query strings, or API parameters. If you insert that input directly into a SQL string, you create a serious security vulnerability called SQL injection.

For example, this kind of code is dangerous:

```
SELECT * FROM users WHERE email = '<user_input>';
```

If `<user_input>` is not handled safely, an attacker can change the meaning of the query. Instead of filtering rows, they can bypass checks or even modify data.

The rules to remember are simple:

- Never build SQL by concatenating user input into strings
- Always use parameterized queries (also called prepared statements)

You will see how to do this properly in [*Chapter 9*](#09.xhtml#_idTextAnchor598), *Django – Web Development on the Backend*, where SQL is generated and executed by application code.

**Good practice**: If a value comes from a user, it must never be inserted directly into a SQL string.

Now that you have practiced reading, let’s move on to modifying it. That means inserting new rows, updating existing rows, and deleting rows.

# Modifying data

This section focuses on changing data. Until now, queries have been read-only. `INSERT`, `UPDATE`, and `DELETE` are the statements that modify what is stored in the database.

A good way to think about it is this: read queries are questions, write queries are decisions.

Start by making a backup copy of `Media.db` so that you can quickly reset if you make a mess of this section. Simply copy the file and name the copied file `Media_backup.db` or similar. (If you forget to make a backup then you can also just recreate the `Media.db` using the `Media.sql` script.)

For each subsection, run the SQL and note its effect on the data by running the following query on a table before and after each modification SQL statement, as shown in the following code:

```
SELECT * FROM <table_name>;
```

## INSERT: adding new rows

`INSERT` creates new rows in a table. At a minimum, you must specify the table and the values for the required columns. Columns that allow `NULL` do not have to be supplied with values.

### Inserting one row

The safest and most readable form explicitly names the columns, as shown in the following code:

```
INSERT INTO titles (name, kind, release_year, universe)
VALUES ('Arrival', 'film', 2016, NULL);
```
