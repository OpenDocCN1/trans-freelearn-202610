+   2. Invoke the `create()` method to create an object in the database in a single step. Ensure that you pass all the required parameters (`first_names`, `last_names`, and `email`):

```
>>> contributor =
    Contributor.objects.create(first_names="Rowel",
    last_names="Atienza",
    email="RowelAtienza@example.com")
```

+   3. Use DB Browser to verify that the contributor record has been created in the database. If DB Browser is not already open, open the `db.sqlite3` database file as we just did in the previous section. Click **Browse Data** and select the desired table – in this case, the `reviews_contributor` table from the Table drop-down menu, as shown in the screenshot – and verify the newly created database record:

![](bbox=[0.13, 0.305, 0.87, 0.425])

Figure 2.21: Verifying the creation of the record in DB Browser

In this exercise, we learned that using the `create()` method, we can create a record for a model in a database in a single step.

# Creating an object with a foreign key

Similar to how we created a record in the `Publisher` and `Contributor` tables, let's now create one for the `Book` table. If you recall, the `Book` model has a foreign key to `Publisher` that cannot have a null value. So, a way to populate the publisher's foreign key is by providing the created `publisher` object in the book's `publisher` field as shown in the following exercise.

# Exercise 2.04 – creating records for a many-to-one relationship

In this exercise, you will create a record in the `Book` table, including a foreign key to the `Publisher` model. As you already know, the relationship between `Book` and `Publisher` is a many-to-one relationship, so you have to first fetch the `Publisher` object and then use it while creating the book record. To do that, follow these steps:

+   1. First, import the `Publisher` class:

```
>>> from reviews.models import Book, Publisher
```