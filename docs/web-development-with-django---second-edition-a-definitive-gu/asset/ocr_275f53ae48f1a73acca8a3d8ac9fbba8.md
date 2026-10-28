You should get the following output:

```
Operations to perform:
  Apply all migrations: reviews
Running migrations:
  Applying reviews.0001_initial... OK
```

This operation creates the database table for the reviews app. The following is a snippet from DB Browser indicating the new reviews_publisher table has been created in the database:

|  | reviews_publisher | CREATE TABLE "reviews_publisher" ("id" integer NOT NULL ... |
|---|---|---|
|  | id | integer | "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT |
|  | name | varchar(50) | "name" varchar(50) NOT NULL |
|  | website | varchar(200) | "website" varchar(200) NOT NULL |
|  | email | varchar(254) | "email" varchar(254) NOT NULL |

Figure 2.13: The reviews_publisher table created after executing the migration command

So far, we have explored how to create a model and migrate it into the database. Let’s now work on creating the rest of the models for our book review application. As we’ve already seen, the application will have the following database tables:

+   - **Book:** This is the database table that holds the information about the book itself. We have already created a Book model and have migrated this to the database.
+   - **Publisher:** This table holds information about the book publisher.
+   - **Contributor:** This table holds information about the contributor, that is, the author, co-author, or editor.
+   - **Review:** This table holds information about the review comments posted by the reviewers.

Let’s add the Book and Contributor models, as shown in the following code snippet, into reviews/models.py:

```
class Book(models.Model):
    """A published book."""
    title = models.CharField(
        max_length=70, help_text="The title of the book.")
    publication_date = models.DateField(
        verbose_name="Date the book was published.")
    isbn = models.CharField(
        max_length=20, verbose_name="ISBN number of the
        book.")
class Contributor(models.Model):
    """A contributor to a Book, e.g. author, editor,
       co-author."""
```