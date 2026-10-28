
There are more buttons in Figure 1.50; from the top, they are **Rerun** (stops the program and restarts it), **Modify Run Configuration...** (opens the run configuration window), **Resume Program** (continues running until the next breakpoint), **Pause Program** (breaks the program at its current execution point), **Stop** (stops the debugger), **View Breakpoints** (opens a window to see all the breakpoints you have set), and **Mute Breakpoints** (which will toggle all breakpoints on or off but not remove them).

9. For now, turn off the breakpoint in PyCharm by clicking on it (the red circle next to line 5).

![](img/4840a43efd335abd10b93708225692d6_82_0.png)

Figure 1.51 – Clicking the breakpoint that was on line 5 disables it

This is just a quick introduction to how to set breakpoints in PyCharm. If you have used debugging features in other IDEs, then you should be familiar with the concepts – you can step through code, step in and out of functions, or evaluate expressions. Once you have set a breakpoint, you can right-click on it to change the options. For example, you can make the breakpoint conditional so that the execution only stops under certain circumstances. All these are beyond the scope of this book but are useful to know about when trying to solve problems in your code.

We’re going to finish the chapter with two activities. First, you’ll build a welcome screen for Bookr, and then you’ll scaffold the book-searching page.

### Activity 1.01 – creating a site welcome screen

The Bookr website that we are building needs to have a splash page that welcomes users and lets them know what site they are on. It will also contain links to other parts of the site, but these will be added in later chapters. For now, you will create a page with a welcome message.

These steps will help you complete the activity:

1. In your `index` view, render the `base.html` template.
2. Update the `base.html` template to contain the welcome message. It should be in both the `<title>` tag in `<head>` and in a new `<h1>` tag in the body.

After completing the activity, you should be able to see something like this:

![](img/4840a43efd335abd10b93708225692d6_83_0.png)

Figure 1.52 – The Bookr welcome page

**Note**

The solution for this activity can be found at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions.

In the next activity, you will build a basic page to show the results of a book search in Bookr.

### Activity 1.02 – a book search scaffold

A useful feature of a site such as Bookr is the ability to search through data to find something on the site quickly. Bookr will implement book searching to allow users to find a particular book by part of its title. While we don’t have any books to find yet, we can still implement a page that shows the text a user searched for. The user enters the search string as part of the URL parameters. We will implement searching and a form for easy text entry in Chapter 6, Forms.

These steps will help you complete the activity:

1. Create a search result HTML template. It should include a variable placeholder to show the search word(s) that were passed in through the render context. Show the passed-in variable in the `<title>` and `<h1>` tags. Use an `<em>` tag around the search text in the body to make it italic.
2. Add a search view function in views.py. The view should read a search string from the URL parameters (in the request’s GET attribute). It should then render the template you created in the previous step, passing in the search value to be substituted, using the context dictionary.
3. Add a URL mapping for your new view to `urls.py`. The URL can be something such as `/book-search`.

After completing this activity, you should be able to pass in a search value through the URL’s parameters and see it rendered on the resulting page. It should look like this:

![](img/4840a43efd335abd10b93708225692d6_84_0.png)

Figure 1.53 – Searching Django workshop

You should also be able to pass in special HTML characters such as < and > to see how Django automatically escapes them in the template.

![](img/4840a43efd335abd10b93708225692d6_84_1.png)

Figure 1.54 – Note how HTML characters are escaped so that we are protected from tag injection

**Note**

The solution for this activity can be found at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions.

### Summary

This chapter was a quick introduction to Django. You first got up to speed on the HTTP protocol and the structure of HTTP requests and responses. We then saw how Django uses the MVT paradigm, and then how it parses a URL, generates an HTTP request, and sends it to a view that returns an HTTP response. We scaffolded the Bookr project and then created the reviews app for it. We then built two example views to illustrate how to get data from a request and use it when rendering templates. You also experimented to see how Django escapes output in HTML when rendering a template.

We did all this with the PyCharm IDE, and you learned how to set it up to debug your application. The debugger will help you find out why things aren’t working as they should.

In the next chapter, you will start to learn about Django’s database integration and its model system, so you can start storing and retrieving real data for your application.

## Models and Migrations

In the previous chapter, we had a brief introduction to Django and its use in developing web applications. Then, we learned about the **Model-View-Template (MVT)** concept. Later, we created a Django project and started the Django development server. We also briefly discussed Django's views, URLs, and templates.

In this chapter, you will be introduced to the concept of databases, the types of databases, and their importance in building web applications. You will start by creating a database using an open source database visualization tool called SQLite DB Browser. You will then perform some basic **create, read, update, and delete (CRUD)** database operations using SQL commands. Then, you will learn about Django's **object-relational mapping (ORM)**, through which your application can interact and seamlessly work with a relational database using simple Python code, eliminating the need to run complex SQL queries. You will learn about models and migrations, which are part of Django's ORM, used to propagate database schema changes from the application to the database, and also perform database CRUD operations. Toward the end of the chapter, you will study the various types of database relationships and use that knowledge to perform queries across related records.

We will cover the following topics in this chapter:

- Understanding and using databases
- Understanding CRUD operations using SQL
- Exploring Django ORM
- Creating Django models and migrations
- Django's database CRUD operations
- Bulk create and bulk update operations
- Performing complex lookups using Q objects
- Activity 2.01 – create models for a project management application
- Populating the Bookr project's database

A database is a structured collection of data that helps manage information easily. A software layer called the **database management system (DBMS)** is used to store, maintain, and perform operations on the data. There are two types of databases: relational databases and non-relational databases. In the following sections, we will learn in brief about relational databases and how data is stored in such databases.

### Relational databases

Relational databases or **structured query language (SQL)** databases store data in a pre-determined structure of rows and columns called **tables**. A database can be made up of more than one such table, and these tables have a fixed structure of attributes, data types, and relations with other tables. For example, as we just saw in *Figure 2.1*, the book inventory table has a fixed structure of columns comprising **Book Number**, **Author**, **Title**, and **Number of Copies**, and the entries form the rows in the table. There could be other tables as well, such as **Student Information** and **Lending Records**, which could be related to the inventory table. Also, whenever a book is lent to a student, the records will be stored per the relationships between multiple tables (say, the **Student Information** and the **Book Inventory** tables).

This pre-determined structure of rules defining the data types, tabular structures, and relationships across different tables acts like scaffolding or a blueprint for a database. This blueprint is collectively called a **database schema**. It will prepare the database to store application data when applied to a database. To manage and maintain these databases, there is a common language for relational databases called SQL. Some examples of relational databases are SQLite, PostgreSQL, MySQL, and OracleDB.

### Non-relational databases

Non-relational databases or **not only SQL (NoSQL)** databases are designed to store unstructured data. They are well suited to large amounts of generated data that does not follow rigid rules, as is the case with relational databases. Some examples of non-relational databases are Cassandra, MongoDB, CouchDB, and Redis.

For example, imagine that you need to store the stock value of companies in a database using Redis. Here, the company name will be stored as the key and the stock value as the value. Using the key-value type NoSQL database in this use case is appropriate because it stores the desired value for a unique key and is faster to access.

For the scope of this book, we will be dealing only with relational databases, as Django does not officially support non-relational databases. However, if you wish to explore, there are many forked projects, such as **Django non-rel**, that support NoSQL databases.

In the next section, you will be introduced to database CRUD operations using SQL commands.
