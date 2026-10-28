# Activity 2.01 – creating models for a project management application

Imagine you are developing a project management application called Juggler. Juggler is an application that can track multiple projects, and each project can have multiple tasks associated with it. The following steps will help you complete this activity:

+   1. Using the techniques we have learned so far, create a Django project called `juggler`.
+   2. Create a Django app called `projectp`.
+   3. Add the app projects to the `juggler/settings.py` file.
+   4. Create two related model classes called Project and Task in `projectp/models.py`.
+   5. Create migration scripts and migrate the models’ definitions to the database.
+   6. Open the Django shell and import the models.
+   7. Populate the database with an example and write a query displaying the list of tasks associated with a given project.

> **Note**
> The full solution for this activity can be found on https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions.

# Populating the Bookr project’s database

Although we know how to create database records for the project, in the next few chapters, we will have to create a lot of records to work with the project. Therefore, we have created a script that can make things easy for us. This script populates the database by reading a comma-separated values (CSV) file consisting of many records. Follow the next few steps to populate the project’s database:

+   1. Create the following folder structure inside the project directory:
+      `bookr/reviews/management/commands/`
+   2. Copy the `loadcsv.py` file from the following location and WebDevWithDjangoData.csv into the folder created. This can be found on the GitHub repository for this book at http://packt.live/3pvbCLM.
+   
+   Because `loadcsv.py` is placed inside the management/commands folder, it now works like a Django custom management command. You can go through the `loadcsv.py` file and read more about writing Django custom management commands here: https://docs.djangoproject.com/en/3.0/howto/custom-management-commands/.