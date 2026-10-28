Now you can see a new entry created in the database using DB Browser:

![](bbox=[0.115, 0.105, 0.85, 0.205])

Figure 2.19: An entry created in the database

+   4. Use the object attributes to make any further changes to the object and save the changes to the database:

```
>>> publisher.email = 'info@packtpub.com'
>>> publisher.email = 'customersupport@packtpub.com'
>>> publisher.save()
```

You can see the changes using DB Browser as follows:

![](bbox=[0.165, 0.35, 0.8, 0.4])

Figure 2.20: An entry with the updated email field

In this exercise, we created an entry in the database by creating an instance of the model object and used the save() method to write the model object into the database.

Note that by following the preceding method, the changes to the class instance are not saved until the save() method is called. However, if we use the create() method, Django saves the changes to the database in a single step. We'll use this method in the exercise that follows.

# Exercise 2.03 – using the create() method to create an entry

Unlike the previous exercise where we executed two separate steps by first creating an object and later using the save() method to create an entry in the database, in this exercise, you will create a record in the contributor table using the create() method in a single step:

+   1. First, import the Contributor class as before:

```
>>> from reviews.models import Contributor
```