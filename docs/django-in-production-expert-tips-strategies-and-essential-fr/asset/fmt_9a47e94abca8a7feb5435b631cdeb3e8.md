
Figure 2.11: The Author object string’s output when we have the `__str__` method defined

## Use custom model methods

Create custom model methods whenever possible to keep your data interface clean. For example, if we have to get the full name of an author, we should use model methods to avoid repetitive business logic in the application layer, like so:

```
class Author(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    email = models.EmailField()
    bio = models.TextField()
    def get_author_name(self):
        return f'{self.first_name} {self.last_name}'
    def get_short_bio(self):
        return f'{self.bio[:200]}...'
```

Here, the `get_short_bio` and `get_author_name` model methods help us retrieve information by just calling the object with the method. For example, `<authorObj>.get_author_name()` would return the full name of the author we have filtered.

### Important note

You shouldn’t add too much logic inside the model method as adding all the business logic to the model can cause it to explode and your code will start to smell. An ideal approach would be to keep a balance and try to identify the limit whenever your model starts to become difficult to test.

Note that model methods only work for single objects, not on QuerySet objects; when you find yourself writing similar filter or ORM queries again and again, you should start considering writing a custom model manager.

We won’t dive into the details of how to implement model managers since the official documentation already goes into this in detail. You can read more about it here: [https://docs.djangoproject.com/en/stable/topics/db/managers/](https://docs.djangoproject.com/en/stable/topics/db/managers/).

## Keep the default primary key

By default, Django gives your auto-increment `Integer` field, which is an `id` field, to all the models you define. For most regular use cases, this ID should be kept as your primary key since it gives you a couple of added advantages, such as filtering entities based on the primary key range, sorting queries based on creation (since the primary key is always auto-incremented), and easy human readability of the primary key.

You should have a strong reason to use an ID field other than the regular `Integer` auto-incremented `id` field. Most applications use UUID or some other unique identifier to replace the existing one. Here are some of the reasons you might want to consider UUID:

* The total number of rows is going to outgrow the `Integer` range
* You don’t want users to guess the next or previous entries using ID

### Important note

In many data-sensitive applications, it is recommended to use both auto ID and UUID, where the default ID can be used internally and UUID can be exposed to the end user.

## Use transactions

By default, Django ORM can commit the data to the database whenever `create()`, `save()`, or `update()` methods are called. When a business use case needs you to update/create multiple rows in a single request, this might become a data corruption problem if transactions are not used.

For example, when a user registers, we have to create a `User` table entry and a `Profile` table entry. However, for some reason, the `User` table entry was created but the backend crashed before the `Profile` table entry was created. Since our application will need both `User` and `Profile` table entries, the user is unable to access our application.

To avoid such scenarios, it is always advisable to wrap queries inside transactions so that whenever there is any issue during the whole process, the whole transaction is rolled back:

```
from django.db import transaction
def viewfunc(request):
    # This code executes in autocommit mode (Django's default).
    # do_db_stuff()
    with transaction.atomic():
        # This code executes inside a transaction.
        pass
        # do_more_db_stuff()
        # do_one_more_db_stuff()
```

In this example, the `do_db_stuff()` function will perform the default auto-commit on any query written inside the function, while the other two functions will only perform commits after both of them are successful.

Read nore

Django provides a wrapper over the database transaction concept. A few important topics any Django developer should know about regarding Django transactions are the `atomic` transaction decorator, `Savepoints` to manage better commit and rollback control, and database-dependent transaction logic – that is, PostgreSQL and MySQL have different transaction locks. You can read more in Django’s official documentation: [https://docs.djangoproject.com/en/stable/topics/db/transactions/](https://docs.djangoproject.com/en/stable/topics/db/transactions/).

## Avoid generic foreign keys

In Django, `GenericForeignKey` is a concept where one model is allowed to have a relationship with any other model in the whole project. Though this is one of the unique features of Django ORM and we have seen this as an interview question in a lot of companies, we should avoid using it in production. Since no constraint is added to the table, it can cause data integrity issues and as the data’s size increases, it causes queries to become slower and slower. `GenericForeignKey` gives us a faster implementation advantage but in the long run, this becomes a bottleneck.

### Read more

The official documentation doesn’t mention all the pitfalls and advantages in detail. Luke Plant, one of the core contributors of Django, narrowed down all the pros and cons of `GenericForeignKey` in this blog: [https://lukeplant.me.uk/blog/posts/avoid-django-genericforeignkey/](https://lukeplant.me.uk/blog/posts/avoid-django-genericforeignkey/).

## Use finite state machines (FSMs)

Most applications will need us to maintain some state for the data. For example, a blogging application would have multiple stages, such as *submitted* > *reviewed* > *published*. The stages should be captured using a database field, `state`. When a new blog is created, the `state` field will have *submitted* as the `state` value. After the blog is thoroughly checked and edited, the state field will have *reviewed* as its value. Finally, when published, the `state` field will have *published* as its value. To programmatically implement this logic in our application, we would have to implement multiple validations and state management.

We can write our own custom logic and validation to handle multiple corner cases, but why should we reinvent the wheel? There is a great package that gives us an interface to just worry about the business logic while it takes care of the underlying infrastructure. I would recommend using Django FSM for any state management-related use case.

### Read more

`django-fsm` is a Django package that helps with simple declarative state management for Django models. Django FSM uses decorators to create a cleaner interface and introduces a new `FSMField`, which is a specialized character field for implementing application state management better. You can learn more about Django FSM here: [https://github.com/viewflow/django-fsm](https://github.com/viewflow/django-fsm).

## Break the model into packages

It is recommended to not have a lot of models inside a single app. However, there can be legacy reasons why new apps were not created and a single model file is 10,000+ lines of code long with 10+ different models. Though there is no such hard limit on how many lines of code a single file can have, it becomes a nightmare for a developer to work on such humongous files. Luckily, Django provides a way to break `models.py` into smaller files and have `models` as a package.

To do so, just create a folder called `models` with an `__init__.py` file, then add all the models you want to add to the package:
