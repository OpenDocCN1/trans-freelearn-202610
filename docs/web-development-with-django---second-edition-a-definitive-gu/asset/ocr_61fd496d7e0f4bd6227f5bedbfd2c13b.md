# django-configurations

One of the main considerations when deploying a Django application to production is how to configure it. As you have seen throughout this book, the `settings.py` file is where all your Django configuration is defined. Even third-party apps have their configuration in this file. You have already seen this in *Chapter 12, Building a REST API*, when working with the Django REST framework.

There are many ways to provide different configurations and switch between them in Django. If you have begun working on an existing application that already has a specific method of switching between configurations in development and production environments, then you should probably keep using that method.

When we release Bookr onto a product web server, in *Chapter 17, Deploying a Django Application (Part 1 - Server Setup)*, we will need to switch to a production configuration, and that's when we will use `django-configurations`.

To install `django-configurations`, use `pip3` as follows:

```
pip3 install django-configurations
```

> **Important note**
> For Windows, you can use `pip` instead of `pip3` in the preceding command.

The output will be as follows:

```
Collecting django-configurations
  Downloading django_configurations-2.4-py3-none-any.whl
    (17 kB)
Requirement already satisfied: django>=3.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django-configurations) (4.0)
Requirement already satisfied: sqlparse>=0.2.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django>=3.2->django-configurations) (0.4.2)
Requirement already satisfied: asgiref<4,>=3.4.1 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django>=3.2->django-configurations) (3.4.1)
Requirement already satisfied: backports.zoneinfo; python_version < "3.9" in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django>=3.2->django-configurations) (0.2.1)
Installing collected packages: django-configurations
Successfully installed django-configurations-2.4
```

`django-configurations` changes your `settings.py` file so that all the settings are read from a class you define, which will be a subclass of `configurations.Configuration`. Instead of the settings being global variables inside `settings.py`, they will be attributes of the class you define. By using this class-based method, we can take advantage of object-oriented paradigms, most