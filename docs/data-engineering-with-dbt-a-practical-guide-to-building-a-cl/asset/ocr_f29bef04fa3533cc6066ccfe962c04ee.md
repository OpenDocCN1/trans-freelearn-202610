# Creating a user for the dbt application

Let’s complete our initial setup by creating a new user that we will use with dbt and grant it the executor role so that it will be able to manage, as an owner, that database.

To create a user, we need to switch to a role with that ability:

+   1. Switch to the USERADMIN role:

```
USE ROLE USERADMIN;
```

+   2. Create the new user for dbt; I have named it DBT_EXECUTOR, though you can pick any name:

```
CREATE USER IF NOT EXISTS DBT_EXECUTOR
    COMMENT = 'User running DBT commands'
    PASSWORD = 'pick_a_password'
    DEFAULT_WAREHOUSE = 'COMPUTE_WH'
    DEFAULT_ROLE = 'DBT_EXECUTOR_ROLE'
;
```

+   3. Let’s assign the executor role to the dbt user:

```
GRANT ROLE DBT_EXECUTOR_ROLE TO USER DBT_EXECUTOR;
```

+   4. Switch back to the operational role, which we should always be working with:

```
USE ROLE DBT_EXECUTOR_ROLE;
```

Great! You have now performed a basic setup of your new Snowflake account; you have learned the basics of user and role management and you are ready to learn more SQL to query data.

# Querying data in SQL – syntax and operators

In this section, you will learn how to query existing data using SQL and the Snowflake example database.

In this chapter, and more generally in this book, you will get familiar with Snowflake syntax, which is modern and very standard. We will also use some proprietary extensions that make your work easier.

# Snowflake query syntax

Snowflake supports querying data with the standard SELECT statement, which has the following basic syntax:

```
WITH ...
SELECT ...
FROM ...
JOIN ...
```