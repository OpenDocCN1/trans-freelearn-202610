
Putting the code from our STG model inside the snapshot slightly sacrifices the ease of building and maintaining the ELT, as you cannot preview the results of the SELECT statement inside the snapshot block.

This lack of ease in building the SELECT query without a preview is at odds with the goal of easily maintaining our models, especially if they implement most of the functionalities that we have been talking about with the STG layer.

We can alleviate this issue by keeping our STG model and declaring it to be an ephemeral model, which is a model that will not generate any persistence (no view or table is created) but only a named query definition. This way, we have an STG model that we can use to preview the results of the query, but we do not need to run it to make it available for the snapshot.

## Layout of snapshot tables

When you use the snapshot functionality, the snapshot table will contain one row for each new version of each instance that the SELECT statement has produced over time. Each time, the most recent version that is (still) active is the current value for the instance identified by the unique key.

The snapshot table will contain the following metadata fields in addition to the fields produced by the SELECT statement:

- dbt_valid_from: The timestamp when the row is inserted in the snapshot. It provides an order for the different “versions” of an instance.
- dbt_valid_to: The timestamp since the row stopped being active. The most recent version of an instance will have dbt_valid_to set to null if it’s still active, or to the highest timestamp for the instance, if it’s not active anymore.
- dbt_scd_id: A unique key generated for each row in the snapshot. This is internal to dbt.
- dbt_updated_at: The timestamp when the row was inserted. This is internal to dbt.

Here are some snapshot best practices:

- Snapshot source data as the first thing in your ETL, declaring a source as appropriate, and make the rest of the ETL depend only on the snapshot.
- Include all the source columns in your snapshot, as you cannot go back in time and backfill information that you do not have, and even if you have it, the backfilling of data is a complicated and time-consuming task.
- Do not add any business logic in the query that feeds data to the snapshot; use only hard rules that do not change the data. If you apply soft business rules and they will change, most probably you will not be able to go back to the original data and apply the new business rule, and even if it’s possible, it would be a complex and slow manual process.
- Avoid joins in your SELECT query, as it is not always easy to establish when the result of a join is a new version, and even more difficult to absorb the changes into two tables. Snapshot the two tables to be joined independently, and then do the join in the downstream models where you apply the business logic.

## Multiple ways to take snapshots into use

We discussed that there are a few ways to take snapshots into use; here is a quick recap:

- **A snapshot as a first step:** Taking a snapshot as the first thing in the flow is what dbt suggests, but it means either preserving the source names by using the trivial SELECT * FROM {{ source(...) }} query or writing a query that is not simple to preview/debug.

  If you use this method, we suggest staying close to the trivial select-star query and then putting a full-fledged STG model just after the snapshot, ensuring that everything else will use the good names and data types, even if the snapshot will not.

- **A snapshot after the STG model:** Taking the snapshot out of an STG model solves the maintainability issue, but it is a bit clumsy if you use a normal model, as you need first to run only the STG models for the snapshots, take the snapshot, and then run all the models except the snapshot ones. The clumsiness is removed if you use ephemeral models for the STG models, as you do not need to run them before.

  If you use this method, you will have the ability to easily write, debug, and test at every run the query that you will use to feed the data to be stored by the snapshot.

Now that we have a good idea of the snapshot functionality, let’s use it.

## Introducing history in our sample project

In our sample project, we get data for the security positions that we have in the fictional ABC Bank.

We have already designed a simple pipeline that allows us to present the data as we prefer and calculate some new information from the input data that we get.

Let’s use the snapshot functionality to store the data that we get and make the overall process more resilient, allowing us to do more with the data that we get over time.

The simple, straightforward way to introduce the snapshot functionality would be to copy and paste the code that we have in the STG model to become the SELECT query in the snapshot. This is possible, but we have already discussed that it would make the development and future management of such a query and the resulting snapshot more complicated, as you cannot preview or test that query in isolation.

To remove that issue, we will change the current STG model to use ephemeral materialization and use it to feed the content of the STG view to the snapshot:

1. First, let’s change our staging model to use an ephemeral materialization – that is, to not produce a database object, but just define a model name for our STG query.

   We just need to add the configuration to make the model use ephemeral:

```sql
{{ config(materialized='ephemeral') }}
```

2. Drop the view created by the STG model before we turned it into ephemeral:

```sql
DROP VIEW <DB>.<SCHEMA>.STG_ABC_BANK_POSITION;
```

   It is better to remove the old view; otherwise, in the future, you or others on the project might think it’s still connected to the STG model and even use it, but the view is not connected to the model anymore and will not be updated if there are changes.

3. Create the snapshot to store the incoming data and its evolution, by entering its configuration and using the STG model as the source of the data. Inside the snapshot folder, create a file named SNSH_ABC_BANK_POSITION.sql, and enter the following code for the snapshot:

```sql
{% snapshot SNSH_ABC_BANK_POSITION %}
{{
    config(
        unique_key= 'POSITION_HKEY',
        strategy='check',
        check_cols=['POSITION_HDIFF'],
        invalidate_hard_deletes=True,
    )
}}
select * from {{ ref('STG_ABC_BANK_POSITION') }}
{% endsnapshot %}
```

In the preceding code of the snapshot, we have the following:

- The snapshot block that defines the snapshot and its name:

  `{% snapshot SNSH_ABC_BANK_POSITION %} ... {% endsnapshot %}`

- The configuration block that defines the behavior of the snapshot. In this case, we have used the following configs:

  - The definition of the field to use as primary key, to identify the instances of the entity that we want to track changes:

    `unique_key = 'POSITION_HKEY'`
  - The definition of the strategy to be used to look for changes – that is, to identify new instances or new versions of a tracked instance:

    `strategy='check'`,
  - `check_cols=['POSITION_HDIFF']`,
  - The configuration to enable tracking of deletions from the source:

    `invalidate_hard_deletes=True`,
  - The query that is executed to fetch the data to update the snapshot:

    `select * from {{ ref ('STG_ABC_BANK_POSITION') }}`

We have used the check strategy, as we do not trust the REPORT_DATE column to work as a good timestamp for the timestamp strategy.

Note that to be able to track deletions, you need to have a reliable full export from the source, as any key that is not in the source is considered deleted.

As we have discussed previously in this section, to work around the impossibility of previewing the query used in a snapshot, we have coded our query in an STG model, configured as ephemeral.

Now that we have created our first snapshot, let’s run it with the following command:

```
dbt snapshot
```

After the run completes in a few seconds, you should find a new table with the name of your snapshot in the database, and the schema that we have configured in the main dbt configuration file, dbt_project.yml.

It is important to remember that snapshots are a global, shared resource, as they end up in the same schema for all the developers and environments that share the same database.

Now that we have the snapshot containing our data, it’s time to connect the REF model with the snapshot in place of the STG model.

## Connecting the REF layer with the snapshot

The REF_POSITION_ABC_BANK model that we wrote before is still reading data from the STG model that we made ephemeral. It still works, but it’s now time to make it read from the snapshot so that our ELT will keep working even if the external source is not available.

If you just change the name of the model inside the ref() function in your REF model, the effect would be to read everything out of the history – that is, all the versions that you encountered in the history of the snapshot. This is not what we want.
