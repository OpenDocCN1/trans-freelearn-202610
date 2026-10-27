```
```jinja
{% endset %}
{% do run_sql(sql) %}  -- if you do not need the results
{% set results = run_query(sql) %}  -- if you need them
```
```

In the preceding snippet, you use a set Jinja block to write or generate the SQL that you want to execute as text, then you call the `run_sql` macro while passing the sql string. You can use the do operator if you do not need the result of your query or the set operator if you want to get hold of the result object.

# Table migrations

Dbt manages most of the DDL and DML commands needed to create and maintain the schemata, tables, and views in your data platform, but in some cases, you might want to do things that dbt does not do for you.

The most common things that you need to manage by yourself are as follows:

+   - **Deleting tables and views that are not in use anymore.**
+   
+     As an example, when you rename a model after running it, the object with the old name will not be deleted by dbt because it cannot know if it is safe to delete it or not. A table could contain useful data or can still be used by other systems.
+   
+   - **Evolving incremental tables.**
+   
+     While normal tables and views are recreated at every run, and therefore any change can be easily applied, the incremental tables are the ones that retain data across executions. Whether you want to rename them or operate on their columns, often, you cannot just drop and recreate them; instead, you have to manually evolve them.
+   
+   - **Performing bulk operations on data.**
+   
+     While editing/tampering with data in a data warehouse should not happen often, there are cases when you will want to edit/fix or delete data. One example might be changing the end-of-time date from 2100-12-31 to 9999-9-9 in a table.

All these operations can be done by writing simple SQL statements, such as `drop table` or `alter table`, which developers should be allowed to run in their development environment, but should not run in shared environments such as QA and PROD.

Once a resource has been published in a shared environment, it becomes part of the public API of your data platform, so you have to manage its evolution and its eventual deletion explicitly, guaranteeing that the changes are aligned across environments and released in sync with the code of each environment.

By using some conventions and some scripting inside macros activated by a run-operation, you can create a simple setup that allows you to quickly write the migrations that you need.