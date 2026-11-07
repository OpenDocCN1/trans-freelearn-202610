To activate the support for views on a server after an upgrade from a pre-5.0 version, the administrator has to execute the mysql_fix_privileges_tables script, as described in the MySQL manual.

> Each user must have the appropriate SHOW_VIEW or CREATE_VIEW privilege to be able to see or manipulate views. These privileges exist at the global (server), database, and table levels.

phpMyAdmin supports two ways of creating a view. We'll explain the manual method first, and then go on with a more visual way.

# Manually creating a view

To create a view manually, we use the query box to enter the appropriate statement. Let's input the following lines and click Go:

```
CREATE VIEW book_author AS
SELECT book.isbn, book.title, author.name FROM book
LEFT JOIN author ON author.id = book.author_id
```

> Creating a view implies that the user has privileges on the tables involved, or at least a privilege such as, SELECT or UPDATE on all the columns mentioned in the view.

At this point, the view has been created, and the left panel is updated to reflect this fact. If we refresh our browser's page and then access the marc_book database, we will see:

![](bbox=[0.19, 0.58, 0.81, 0.7])

In the left panel, there is a different icon next to the book_author view indicating it's not a regular table. In the right panel, we see the information on newly-created view. The number of records for the view currently indicates 0 (more on this in the Performance Hint section later in this chapter), and View is indicated in the Type column. There is no collation or size associated with a view.