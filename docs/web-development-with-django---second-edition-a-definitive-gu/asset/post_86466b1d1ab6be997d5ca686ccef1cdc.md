
You should get the following output:

```
Operations to perform:
  Apply all migrations: reviews
Running migrations:
  Applying reviews.0001_initial... OK
```

This operation creates the database table for the reviews app. The following is a snippet from DB Browser indicating the new reviews_publisher table has been created in the database:

|  | 名称 | 类型 | 数据库定义 |
|---|---|---|---|
|  | reviews_publisher |  | CREATE TABLE "reviews_publisher" ("id" integer NOT NULL ... |
|  | id | integer | "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT |
|  | name | varchar(50) | "name" varchar(50) NOT NULL |
|  | website | varchar(200) | "website" varchar(200) NOT NULL |
|  | email | varchar(254) | "email" varchar(254) NOT NULL |

Figure 2.13: The reviews_publisher table created after executing the migration command

So far, we have explored how to create a model and migrate it into the database. Let’s now work on creating the rest of the models for our book review application. As we’ve already seen, the application will have the following database tables:

- **Book:** This is the database table that holds the information about the book itself. We have already created a Book model and have migrated this to the database.
- **Publisher:** This table holds information about the book publisher.
- **Contributor:** This table holds information about the contributor, that is, the author, co-author, or editor.
- **Review:** This table holds information about the review comments posted by the reviewers.

Let’s add the Book and Contributor models, as shown in the following code snippet, into reviews/models.py:

```
class Book(models.Model):
    """A published book."""
    title = models.CharField(
        max_length=70, help_text="The title of the book.")
    publication_date = models.DateField(
        verbose_name="Date the book was published.")
    isbn = models.CharField(
        max_length=20, verbose_name="ISBN number of the book.")
class Contributor(models.Model):
    """A contributor to a Book, e.g. author, editor,
       co-author."""
    first_names = models.CharField(
        max_length=50, help_text="The contributor's first name or names.")
    last_names = models.CharField(
        max_length=50, help_text="The contributor's last name or names.")
    email = models.EmailField(
        help_text="The contact email for the contributor.")
```

The code is self-explanatory. The Book model has the title, publication_date, and isbn fields. The Contributor model has the first_names and last_names fields and the email ID of the contributor. There are also some newly added models, apart from the ones we have seen in the Publisher model. They have DateField as a new field type, which, as the name suggests, is used to store a date. A new field option called verbose_name is also used. It provides a descriptive name for the field. Next, we will see how relationships work in a relational database.

## Relationships

One of the powers of relational databases is the ability to establish relationships between data stored across database tables. Relationships help maintain data integrity by establishing the correct references across tables, which in turn helps maintain the database. Relationship rules, on the other hand, ensure data consistency and prevent duplicates.

In a relational database, there can be the following types of relations:

- Many-to-one
- Many-to-many
- One-to-one

Let's explore each relationship in detail in the following sections.

### Many-to-one

In this relationship, many records (rows/entries) from one table can refer to one record (row/entry) in another table. For example, there can be many books produced by one publisher. This is an example of a many-to-one relationship. To establish this relationship, we need to use the database's foreign keys. A foreign key in a relational database establishes the relationship between a field from one table and a primary key from a different table.

For example, say you have data about employees belonging to different departments stored in a table called employee_info with their employee ID as the primary key alongside a column that stores their department name; this table also contains a column that stores that department's department ID. Now, there's another table called departments_info, which has the department ID as the primary key. In this case, then, the department ID is a foreign key in the employee_info table.

In our bookr app, the Book model can have a foreign key referring to the primary key of the Publisher table. Since we have already created the models for Book, Contributor, and Publisher, let’s now establish a many-to-one relationship across the Book and Publisher models. For the Book model, add the last line:

```
class Book(models.Model):
    """A published book."""
    title = models.CharField(
        max_length=70, help_text="The title of the book.")
    publication_date = models.DateField(
        verbose_name="Date the book was published.")
    isbn = models.CharField(
        max_length=20, verbose_name="ISBN number of the book.")
    publisher = models.ForeignKey(
        Publisher, on_delete=models.CASCADE)
```

Now the newly added publisher field establishes a many-to-one relationship between Book and Publisher using a foreign key. This relationship ensures the nature of a many-to-one relationship, which is that many books can have one publisher:

- models.ForeignKey: This is the field option to establish a many-to-one relationship.
- Publisher: When we establish relationships with different tables in Django, we refer to the model that creates the table; in this case, the Publisher table is created by the Publisher model (or the Publisher Python class).
- on_delete: This is a field option that determines the action to be taken upon the deletion of the referenced object. In this case, the on_delete option is set to CASCADE (models.CASCADE), which deletes the referenced objects.

For example, assume a publisher has published a set of books. For some reason, if the publisher has to be deleted from the application, the next action is CASCADE, which means deleting all the referenced books from the application. There are many more on_delete actions, such as the following:

- PROTECT: This prevents the record deletion unless all the referenced objects are deleted
- SET_NULL: This sets a null value if the database field has been previously configured to store null values
- SET_DEFAULT: Sets to a default value on the deletion of the referenced object

We will only use the CASCADE option for our book review application.

### Many-to-many

In this relationship, multiple records in a table can have a relationship with multiple records in a different table. For example, a book can have multiple co-authors, and each author (contributor) may have written multiple books. So, this forms a many-to-many relationship between the Book and Contributor tables:

![](img/4840a43efd335abd10b93708225692d6_110_0.png)

Figure 2.14: Many-to-many relationship between books and co-authors

In models.py, for the Book model, add the last line as shown here:

```
class Book(models.Model):
    """A published book."""
    title = models.CharField(
        max_length=70, help_text="The title of the book.")
    publication_date = models.DateField(
        verbose_name="Date the book was published.")
    isbn = models.CharField(
        max_length=20, verbose_name="ISBN number of the book.")
    publisher = models.ForeignKey(
        Publisher, on_delete=models.CASCADE)
    contributors = models.ManyToManyField(
        'Contributor', through="BookContributor")
```

The newly added contributors field establishes a many-to-many relationship with Book and Contributor using the ManyToManyField field type:

- models.ManyToManyField: This is the field type to establish a many-to-many relationship.

- through: This is a special field option for many-to-many relationships. When we have a many-to-many relationship across two tables, if we want to store some extra information about the relationship, then we can use this to establish the relationship via an intermediary table.
