Now, let’s see what happens behind the scenes when we create a new database. PostgreSQL performs the following steps:

+   1. Makes a physical copy of the template database, `template1`
+   2. Assigns the database name to the database just copied

The template1 database is a database that is created by the initdb process during the initialization of the PostgreSQL cluster.

# Managing databases

We’ve just seen how to create databases. In this section, we will see how to manage databases, how to list all the databases present on a cluster, how to create a database starting from an existing database, how to drop a database, and what happens internally, behind the scenes, when we create and drop a database.

# Introducing schemas

As reported in Chapter 1, Introduction to PostgreSQL: “a database can be organized into namespaces, called schemas. A schema is a mnemonic name that the user can assign to organize database objects, such as tables, into a more structured collection. Schemas cannot be nested, so they represent a flat namespace.” Referring again to Chapter 1, Introduction to PostgreSQL, we’ve learned that there are two kinds of users, normal users and superusers:

+   - Superusers can do everything across databases and schemas.
+   - Normal users can do operations depending on their privilege set.

# PostgreSQL and the public schema

Starting from PostgreSQL 15, PostgreSQL has changed the way to manage the public schema. In this section, we will see how it works. Before PostgreSQL 15, any user was able to perform any DDL operation on the public schema. PostgreSQL 15 introduces the concept of removing global privileges from the public schema.

Starting from PostgreSQL 15:

+   - A normal user will not be able to execute DDL on the public schema.
+   - A normal user will not be able to perform DML on the public schema unless they receive permission from a superuser.

Let’s use an example to better explain how this new feature works. We will work as if we were on a PostgreSQL version <=14.x.