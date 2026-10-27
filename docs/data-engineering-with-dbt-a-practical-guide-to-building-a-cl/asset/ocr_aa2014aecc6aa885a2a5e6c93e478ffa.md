# Users and roles

To control access to your data in SQL, you GRANT access and other privileges to both users and roles.

A user represent one individual user or service that can access the database, while a role represents a named entity that can be granted privileges.

A role can be granted to a user, providing them all the privileges associated with the role.

A role can also be granted to another role, building hierarchies that use simple basic roles to build more complex roles that are assigned to a user.

Using roles instead of granting privileges directly to users allows you to manage even a large number of users simply and consistently. In this case, roles are labels for a set of privileges and become a way to manage groups of users that we want to grant the same privileges. Changing the privileges granted to a role at any moment will change the privileges that the users receive from that role.

A typical pattern when working with dbt is to create a role for the dbt users and then assign it to the developers and the service user that the dbt program will use.

The following is an example of a simple setup with one role and a couple of users:

```
CREATE ROLE DBT_SAMPLE_ROLE;
CREATE USER MY_NAME;          -- Personal user
CREATE USER SAMPLE_SERVICE;   -- Service user
GRANT ROLE DBT_SAMPLE_ROLE TO USER MY_NAME;
GRANT ROLE DBT_SAMPLE_ROLE TO USER SAMPLE_SERVICE;
```

A more complex setup could have one role to read and one to write for each source system (represented by a schema with the data from the system), for the data warehouse (one or more schemata where the data is processed), and for each data mart (one schema for each data mart).

You could then control in much more detail who can read and write what, at the cost of more effort.

# Understanding the categories of SQL commands

SQL commands can be organized into categories according to their usage in the language:

+   - Data Definition Language (DDL): DDL contains the commands that are used to manage the structure and organization of a database

+   - Data Manipulation Language (DML): DML contains the commands that are used to manipulate data, for example, INSERT, DELETE, and UPDATE

+   - Data Query Language (DQL): DQL contains the SELECT command and is the central part of SQL that allows querying and transforming the data stored in a database