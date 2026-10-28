```
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
]
```

These are a set of installed or default apps used for the admin site, authentication, content types, sessions, messaging, and an application to collect and manage static files. In the upcoming chapters, we shall study this in depth. For the scope of this chapter, though, we shall understand why Django migration is needed for these installed apps in the following section.

# Django migration

As we learned before, Django’s ORM helps make database operations simpler. A major part of the operation is transforming the Python code into database structures, such as database fields with stated data types and tables. In other words, the transformation of Python code into database structures is known as **migration**. For example, instead of creating database tables by running SQL commands, you would write models for them in Python; something you’ll learn to do in the upcoming *Creating models and migrations* section. These models will have **fields** that form the blueprints of database tables. The fields, in turn, will have different field types giving us more information about the type of data stored there (recall how we specified the data type of our field as TEXT in step 4 of Exercise 2.01 – creating a book database).

Since we have a Django project set up, let’s perform our first migration. Although we have not added any code yet to our project, we can migrate the applications listed in INSTALLED_APPS. This is necessary because Django’s installed apps need to store the relevant data in the database for their operations, and migration will create the required database tables to store the data in the database. The following command should be entered in the terminal or shell to do this:

```
python manage.py migrate
```

> **Note**
> For macOS, you can use python3 instead of python in the preceding command.

Here, manage.py is a script that was automatically created when the project was created. It is used for carrying out managerial or administrative tasks. By executing this command, we create all the database structures required by the installed apps.

As we are using DB Browser for SQLite to browse the database, let’s take a look at the database for which changes have been made after executing the migrate command.