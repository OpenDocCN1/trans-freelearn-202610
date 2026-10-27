+   - For each migration to run, there is a `log` and a `run` statement. The log is useful to show in the log the migration being run and the database and schema used.
+   - There is a commented-out `log` statement to use when there are no migrations.

We have also created a sample migration, named `V001_drop_example_table`, to show the naming convention and how simple it becomes to write additional migrations and add them to the `run_migrations` macro:

```
{% macro V001_drop_example_table(
    database = target.database,
    schema_prefix = target.schema
) -%}
DROP TABLE IF EXISTS
{{database}}.{{schema_prefix}}_STAGING.TABLE_XXX ;
{%- endmacro %}
```

The preceding sample migration has the same arguments as the `run_migrations` macro and exemplifies the use of the arguments to run the SQL command in the correct database and schema according to the environment in use.

By using this simple setup, all your developers can add migrations that will evolve the tables in sync with the release of the code that uses the new names of tables and columns, keeping the release process very smooth.

Once a migration has done what it was written for in all the needed environments it can be removed from the `run_migrations` macro and the file can be moved to the ARCHIVED folder.

# Documentation

A great advantage of working with dbt is the documentation that is produced automatically or with minimal effort by the developers.

In this section, we will touch on the following documentation-related features:

+   - **Lineage graph**: This is the graph of dependencies between the models in our project that dbt presents us with in the IDE and the generated documentation
+   - **Generated documentation**: This is an automatically generated website describing all the models using human-provided and generated metadata
+   - **Source freshness report**: This is a web page that checks how fresh the data in the sources is and verifies it against the SLA set in the YAML files
+   - **Exposures**: This feature allows us to describe the dependencies of external objects such as dashboards or ML models on dbt-managed models