
## From Object-Relational Mapping to MVC Containers

This chapter continues to prepare you to use Laminas. Here, we will go through the last two stages:

- The handling of databases with object-relational mapping (ORM), abstracting SQL statements
- The inversion of control for object-oriented PHP applications with the Laminas framework as a container

First, we will explain the foundations of ORM implemented with the aid of laminas-db. Then, we will introduce you to the basics of Laminas by building an MVC application. These foundations are very important for the creation of an e-commerce application and learning them will be invaluable. You could jump to Chapter 5, Creating the Virtual Store Project, but you may feel like you are trying to attack the Death Star in a hang glider. Keep calm and prepare yourself for battle.

In this chapter, we'll be covering the following topics:

- Using ORM
- Use case – managing students
- Using Laminas as a container

### Technical requirements

All the code related to this chapter can be found at https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter04.

### Using ORM

You have learned how to connect to a relational database with `laminas-db` using its implementation of the **Adapter** design pattern. However, although we created a structure that allows switching between different SQL database vendors through configuration, we kept writing SQL statements. When we write SQL statements directly inside the code of an application, we have to choose a specific SQL dialect. Although there is a SQL standard, the fact is that there are some differences, such as the use of quotations and the names of functions.

For example, in MySQL, a query on the `resources` table of the `whatstore` database would have this form:

```
SELECT `code`, `name`, `method` FROM `resources`
```

On the other hand, the same query in PostgreSQL would look like this:

```
SELECT "code","name","method" FROM "public"."resources"
```

As we can see, the quotation character is different for each one of these vendors. If you use the `SELECT` query of one of them in the other, an error will occur. Imagine changing several files of an application because you are migrating from one vendor to another.

You might say that some string replacement based on a regular expression could solve this fast. However, in addition to the quotation issue, there are differences between database functions. For example, MySQL aggregates values of fields from several table records, like this:

```
SELECT GROUP_CONCAT(`name`) as `names` FROM `resources`
```

But PostgreSQL uses another function for the same result:

```
SELECT string_agg("name",',') AS "names" FROM "public"."resources"
```

So, database migration can be a hard task if the application directly writes the SQL statements.

ORM avoids the rewriting of code if you need to migrate from one database vendor to another. This facility is possible thanks to SQL statement encapsulation.

To get started with ORM, we will learn how to deal with tables as objects. The `laminas-db` component implements the **Table Data Gateway** and **Row Data Gateway** design patterns, which allow access to a table or a record set as an object and allow access to a single record as an object, respectively. In this chapter, we will use the Table Data Gateway pattern.

### Table Data Gateway

Martin Fowler, in his book *Patterns of Enterprise Application Architecture*, Addison-Wesley, defines Table Data Gateway as “an object that acts as a Gateway to a database table.” This object is intended to handle several rows from a table. Gateway is also the name of a design pattern. Martin Fowler, in the same book, defines a Gateway as “an object that encapsulates access to an external system or resource.” Databases are external systems for web applications, so it makes sense that some object-relational patterns extend the gateway pattern. You can search for one single record with a Table Data Gateway, but this object always returns a record set (in this case, a set with one only element).

### Row Data Gateway

Martin Fowler, in his book *Patterns of Enterprise Application Architecture*, Addison-Wesley, defines Row Data Gateway as “an object that acts as a Gateway to a single record in a data source.” While Table Data Gateway focuses on a record set, Row Data Gateway focuses on a single record.

We will create a `school12` project in this chapter, which will implement ORM with `laminas-db`. This `school12` project has a difference, related to the separation of responsibilities, from the `school11` project. In `school11`, the same class (`SchoolClass`) stores the business data and deals with communication with the database. Now, we will have a class to store the data in memory that doesn’t know the data source and another class to read and write to the database. This approach avoids coupling the business rules with the data storage model. We can separate responsibilities in the following way:

1. Make a copy of the `school11` project in the `htdocs` folder and name it `school12` in the same `htdocs` folder. Remember that we agreed in *Chapter 2, Setting Up the Environment for Our E-Commerce Application*, that the `htdocs` folder would be our workspace throughout the book. You can do this by selecting the `school11` project with the right mouse button and clicking on **Copy**, or simply pressing `Ctrl + C`. Then, press `Ctrl + V` and you will see a dialog box, as in Figure 4.1:

![](img/e6cf5550804ccae9d1859270da8688f5_95_0.png)

Figure 4.1 – Dialog box for copying a project

2. Make sure the **Project name** field is filled with `school12`; then, click on **Copy**.

3. Select the `school11` project with the right mouse button and click on the **Close Project** item.

We are closing our first project because we don't need it anymore and if it stays open, we may accidentally change a file in that project thinking it belongs to the `school12` project. From now on, we will work on the `school12` project.

### Use case – managing students

In this second project (`school12`), we will implement one more use case, *managing students*. This use case is very similar to *managing school classes*. Both have four flows, related to create, recover, update, and delete actions. The difference is that the `student` model is dependent on the `school class` model. So, a precondition for the flows of the *managing students* use case is that the `classes` table must have at least one record.

### Implementing the use case

By now, you know how to create folders. So, let us implement a use case by following these steps:

1. Create a new folder named `view` in the root of the project.

2. Inside this folder, create two more folders: `schoolclass` and `student`.

The `view` folder is an explicit division for files related to the user interface. This organization will help us to identify what belongs to the presentation layer.

3. Next, select the following files:

- `delete.php`
- `edit.php`
- `index.php`
- `list.php`
- `save.php`

4. Copy these files to the `view/schoolclass` and `view/student` folders.

5. Now, you can remove all of them from the project root, except for the `index.php` file. The `index.php` file is the starting point of the application and will act as a coordinator for the other files. The `index.php` file is akin to the front door of a house. You need to get into the house to access the rooms. In a similar way, a user will have to use `index.php` before they access the other PHP files.

6. Replace the content of this index.php file with the following source code:

### index.php

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>School Management</title>
</head>
<body>
<h1>School Management</h1>
<ul>
<li><a href="view/schoolclass">School classes
 </a></li>
<li><a href="view/student">Students</a></li>
</ul>
</body>
</html>
```

As you can see, the home page is a static page that has links for the two registrations. At this stage, the index.php file is purely presentational. Now, we will refactor the other presentation files. Let's start with the use case we have implemented in the school1 project, managing school classes.

We need to change the path for importing the autoload.php file in the files inside the view/schoolclass and view/student folders. As the files inside these folders were in the application root folder before, the path is now wrong. Inside the view/schoolclass and view/student folders, all files that import autoload.php must have, from now on, the following line at the beginning:

```
require realpath( __DIR__ . '/../../../vendor' ) . '/autoload.php';
```

If you don't make this change, you won't be surprised by an error related to this path. The complete source code is provided, but it's good practice to code instead of copying and pasting. When you type, you can make mistakes and need to review the code you have typed to avoid errors. This process helps to assimilate the content. It is very important to know what you are doing; otherwise, you will only be a “code operator.”

These first steps give us the general outline of our new project structure. We have two registrations, one for school classes and another for students. Let's start by refactoring the code for the school classes, which we have inherited from the school1 project. Remember that test classes remain in the same place.
