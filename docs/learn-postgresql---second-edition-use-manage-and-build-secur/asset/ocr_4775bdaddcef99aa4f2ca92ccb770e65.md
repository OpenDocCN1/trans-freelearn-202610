The following are the steps that we will execute (some instructions will be explained later in this book):

+   1. We will create a normal user called myuser.
+   2. We will connect to the database as the user myuser.
+   3. As myuser, we will try to create a new table called mytable.

Below, you will find the execution of what is written above:

```
forumdb=# create user myuser with password 'SuperSecret' login;
CREATE ROLE
forumdb=# set role to myuser;
SET
forumdb=> create table mytable(id integer);
ERROR:  permission denied for schema public
LINE 1: create table mytable(id integer);
```

As we can see, a normal user cannot create a table (DDL) on a public schema.

# The search_path variable

PostgreSQL has many system variables. One of them is called search_path. The search_path variable contains the sequence of schemas that PostgreSQL uses to find tables; the search_path default value is $user, public. This means that first it will search all the tables in the schema that have that name in the user table and then it will search the public schema.

For example, if we have a user called forum, and we want to show all the records that are present in a table called cities, first PostgreSQL will search the cities table in the forum schema, and if the cities table cannot be found in the forum schema, PostgreSQL will search for the cities table in the public schema.

# The correct way to start working

Let’s start from scratch and execute the following steps:

+   1. As a superuser, let’s create a new database called myforumdb and connect to it.
+   2. As a superuser, let’s create a new user called myforum.
+   3. As a superuser, let’s create a new schema called myforum with authorization for the myforum user.