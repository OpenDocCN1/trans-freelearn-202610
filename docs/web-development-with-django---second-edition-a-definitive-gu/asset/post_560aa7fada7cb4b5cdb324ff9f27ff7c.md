
Typically, here is how a bulk_create method is called by supplying a list of objects:

```
Person.objects.bulk_create([
    Person(name='Robert', address='5, Byron bay, NSW'),
    Person(name='Mark', address="Unit 12, New town, NSW 2000"),])
```

Likewise, the bulk update is also performed on a list of similar objects. The object's attributes can be changed as shown in the following example and updated in the database in a single command using the bulk_update() method, as shown in the following snippet:

```
persons[0].address = "8, Byron bay, NSW"
persons[1].address = "15, New town, NSW"
Person.objects.bulk_update(persons, ["address"])
```

We will see further examples of both of these in the following exercises.

## Exercise 2.15 – creating multiple records using bulk_create

In this exercise, we will use the bulk_create() method to create multiple entries into the Publisher table in one go. To do that, follow these steps:

- 1. Import the Publisher model if you have not already imported it into your Django shell:

```
>>> from reviews.models import Publisher
```

- 2. Create multiple records in the Publisher table by passing a list of the Publisher objects into the bulk_create() method:

```
>>> Publisher.objects.bulk_create([
    Publisher(name="New Town Publisher",
              website="www.newtownexample.com",
              email='newtow@example.com'), Publisher(name="Byron Bay Press", website="www.byronbayexample.com", email='byronbayexample@email.com'),
    Publisher(name="Katoomba Publisher",
              website="www.katoombaexample.com",
              email='katoombaexample@email.com')])

[<Publisher: New Town Publisher>, <Publisher: Byron Bay Press>, <Publisher: Katoomba Publisher>]
```

The method returns a list of objects created as entries in the database. If a large number of entries need to be created, this might be one of the most efficient ways of carrying out the task.

Next, we shall try to bulk update records efficiently using the bulk_update method.

## Exercise 2.16 – updating multiple records using `bulk_update`

Similar to the previous exercise, we can update multiple records in one go by using the `bulk_update` method:

- 1. First, import the `Publisher` model if you have not already imported it:

```
```python
>>> from reviews.models import Publisher
```
```

- 2. In the next step, we will pick a couple of `Publisher` objects as a list.

- 3. Assuming both of these publishers were merged into a single company, and they need to share the same website, now let us update both the objects in a query set:

```
```python
>>> publishers = [Publisher.objects.get(name='New Town Publisher'), Publisher.objects.get(name='Byron Bay Press')]
>>> publishers[0].website = "www.newsouthwalespublisher.com"
>>> publishers[1].website = "www.newsouthwalespublisher.com"

>>> Publisher.objects.bulk_update(publishers, ["website"])
2
```
```

Upon running the `bulk_update` method, it returns the number of objects updated; in this case, it is two objects. Again, this is an efficient way to update more than one object in one go.

## Performing complex lookups using Q objects

Q objects are used to perform complex queries especially when a query involves the AND or OR operations in a WHERE clause. For instance, if we need to do a query similar to the SQL query shown here:

```
```sql
SELECT * FROM Person WHERE name LIKE "Rob%" OR name LIKE "Bob%";
```
```

The preceding SQL statement queries for any person whose name either starts with Rob or Bob.

In `LIKE "Rob%"`, Here, the `LIKE` keyword pattern-matches a string to check whether the string starts with the specified Rob value.

Q objects use the & and | operators for the AND and OR operations when combining the WHERE clauses. The preceding query can be written as follows using Q objects:

```
Person.objects.get(Q(name__startswith='Rob') | Q(name__startswith='Bob'))
```

## Exercise 2.17 – performing a complex query using a Q object

Next, using the concept learned before, Q objects let us perform a query for two of the publishers:

- 1. Import Publisher if you have not already done so and also import Q from django.db.models, as shown in the following command:

```
>>> from django.db.models import Q
>>> from reviews.models import Publisher
```

- 2. Execute the Q object query as shown next. Here, we check whether any of the Publisher objects has name starting with either New or Idea:

```
>>> Publisher.objects.filter(Q(name__startswith="New")
        | Q(name__startswith="Idea"))
<QuerySet [<Publisher: New Town Publisher>]>
```

Since there is no Publisher with name starting with Idea and there was an object with name starting with New, one object was returned.

This is an example Q object query with the AND operator. In this query, we query a Publisher object whose name starts with New and also whose name ends with Publisher:

```
>>> Publisher.objects.filter(Q(name__startswith="New") &
        Q(name__endswith="Publisher"))
<QuerySet [<Publisher: New Town Publisher>]>
```

In this case, only one object was returned, which is New Town Publisher, which satisfies the conditions stated in the query. This way, multiple such WHERE clauses can be combined using the AND or OR operators to get the desired results.

## Exercise 2.18 – verifying whether a queryset contains a given object

In this exercise, we will use a contains method to check whether the query contains a specified object:

- 1. Import the Publisher model if you have not already imported it:

```
>>> from reviews.models import Publisher
```

- 2. Run a query, as shown next. In this example, we are executing a Q object query, and the result has two publishers:

```
```python
>>> publishers =
    Publisher.objects.filter(Q(name__startswith="New")
        | Q(name__endswith="Publisher"))
>>> publishers
<QuerySet [<Publisher: New Town Publisher>, <Publisher: Katoomba Publisher>]>
```
```

- 3. Fetch a new publisher object and verify whether it is a part of the previous query set:

```
```python
>>> new_town_publisher =
    Publisher.objects.get(name='New Town Publisher')
>>> new_town_publisher
<Publisher: New Town Publisher>
>>> publishers.contains(new_town_publisher)
True
```
```

If the object is present, it returns a Boolean value such as `True` or `False`.

The following is an example when a certain query set does not have the specified object, in which case the result returned is `False`:

```
```python
>>> publishers.contains(Publisher.objects.get(name='Byron
    Bay Press'))
False
```
```

In this exercise, we learned how a simple method, `contains()`, can be used to check whether an object is part of a query set.

Overall, in this entire section, we learned about and used Django’s command-line interactive shell. Using the models created in the previous section for the book review application, we used the Django interactive shell to perform CRUD operations. We also explored various ways to filter and query (Read) from the database. All of these database operations will come in handy while developing any Django application. Using the concepts learned so far in this chapter, in the next section, you will create models for a sample application and perform some of the CRUD operations.

## Activity 2.01 – creating models for a project management application

Imagine you are developing a project management application called Juggler. Juggler is an application that can track multiple projects, and each project can have multiple tasks associated with it. The following steps will help you complete this activity:

- 1. Using the techniques we have learned so far, create a Django project called `juggler`.
- 2. Create a Django app called `projectp`.
- 3. Add the app projects to the `juggler/settings.py` file.
- 4. Create two related model classes called Project and Task in `projectp/models.py`.
- 5. Create migration scripts and migrate the models’ definitions to the database.
- 6. Open the Django shell and import the models.
- 7. Populate the database with an example and write a query displaying the list of tasks associated with a given project.

> **Note**
> The full solution for this activity can be found on https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions.

## Populating the Bookr project’s database

Although we know how to create database records for the project, in the next few chapters, we will have to create a lot of records to work with the project. Therefore, we have created a script that can make things easy for us. This script populates the database by reading a comma-separated values (CSV) file consisting of many records. Follow the next few steps to populate the project’s database:

- 1. Create the following folder structure inside the project directory:
- `bookr/reviews/management/commands/`
- 2. Copy the `loadcsv.py` file from the following location and WebDevWithDjangoData.csv into the folder created. This can be found on the GitHub repository for this book at http://packt.live/3pvbCLM.

Because `loadcsv.py` is placed inside the management/commands folder, it now works like a Django custom management command. You can go through the `loadcsv.py` file and read more about writing Django custom management commands here: https://docs.djangoproject.com/en/3.0/howto/custom-management-commands/.
