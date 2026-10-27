
- Dealing with source system exports and understanding well how each system provides its data for loading allows you to assess whether you can capture deletions of data or not, and it will tell you at what resolution you will be able to capture the changes.
- Separating source system identity management (to store history) from master data management (to produce integrated information) will show you where you are missing information and will surface the need to actively manage the connections between systems.

We feel that being compelled to deal with these topics early, one entity at a time, is a big positive, as it saves you from having to continue with ambiguous entities that are not clearly understood.

Without such clarity, you might produce something that initially works, until you have a small project with a single developer being consistent with his implicit assumptions, but it is extremely improbable that you might achieve any positive result in a medium-to-complex project with a few developers, let alone create an easy-to-maintain, long-lasting platform.

As an example, in our sample project, we have the position entity, and when we picked a key for it, we saw that its identity was not set in stone and that we could pick a primary key that we feel comfortable with, but we might change that later. This is often the case with facts.

The point here is that we are in a sample project, and there is no real business in the background to discuss and evaluate the different possibilities or just to provide us with some business context, so we must go with gut feelings and experience, which might prove sub-optimal and different for each developer.

In a real use case, the business will be able to answer your questions about the implications of picking one key or another, to better point you toward an appropriate decision.

## Saving history with dbt

When working with dbt, we have two main avenues to pick up and save the changes to the instances of our entities:

- *Using the standard functionality of dbt snapshots*: This allows you to easily capture changes as a **Slowly Changing Dimension of type 2 (SCD2)** without writing any code for that purpose, as the logic is implemented by dbt itself using merge or delete/insert statements. If we get reliable full exports from the source, the snapshot can also capture deletions.

  This works very well for normal-size datasets (up to millions of rows), and it’s the simplest way to go, but it comes with its own cons, so we generally prefer to store changes using insert only history, as explained in the second point.

  One of the major cons of snapshots is that they are global objects, so you only have one copy and one version active for all environments. Another one is that you cannot preview or test the SELECT query that feeds the data to the snapshot.

  As you can imagine, these are not big issues if you have a simple query such as `SELECT * FROM <source>`, but it is a limitation if you want to apply the best practices that we have discussed for the staging layer before storing the history.

- *Using incremental models to save the new or changed data from each run:* We prefer to use this approach, applying set-based operations to capture changes in insert-only mode, as this is the most effective way to manage and store even huge amounts of data, and it can be tailored to work in all cases, even when you have multiple sources for a single table, such as a near-real-time feed and a periodic control feed.

It requires writing your own macro, or adopting our own, to implement the logic to store the history – that is, to check whether the version of one instance present in the staging model is a new instance or a change relating to the last stored version of that instance.

This is harder to explain than do in SQL, and we already have the macro ready, so for us, this method will amount to less work and allow us to store the changes exactly how we want. We will present the macro and this method in the final chapters, together with the more ingestion-related topics.

In the next section, we will introduce you to the use of snapshots to store changes and, eventually, also record deletions.

## Saving history using snapshots

dbt provides the snapshot functionality to save incoming data. The best use of it is to capture incoming source data very close to how the source is, that is with the least changes possible.

In a snapshot, it is perfectly possible to apply all the transformations that we have discussed for the staging model. Doing that in a snapshot works well in most cases, but it is just a bit less transparent than doing it in a STG model, as we will see in the next chapter.

### The snapshot functionality

A snapshot is a dbt-managed table that will store the data from a SELECT statement in the form of a slowly changing dimension.

A snapshot is created by the special `snapshot` block that you create in a `.sql` file in dbt.

Snapshots are not normal models, in the sense that they are not run with the `dbt run` command, but they must be run with the specific `dbt snapshot` command or with the catch-all `dbt build` command.

The snapshot tables are usually generated in their own specific schema, as they are the only tables that cannot be recreated at every run, so you do not want to drop them by accident. Because of that, they are stored in the snapshots folder and not in the models folder.

The **check** strategy is the one that you use when you do not have a good high-watermark indicator, and instead, you must look at the data to check whether each row presented by the SELECT statement is the same or a different instance from the most recent one stored in the snapshot under the specified unique key. With this strategy, you must configure the columns to be checked to decide whether two rows are the same instance or not, using the `check_cols` configuration. You do not have to list all columns from SELECT but only the ones you want to compare; in any case, all columns from SELECT will be stored in the snapshot.

Once you have defined one or more snapshots, you run them with the `dbt snapshot` command, and you can reference the data in the snapshot in other models using the normal `ref` function, by using the snapshot name as the referenced model, as in `ref('snapshot_name')`.

The intended usage of snapshots is as starting point of your ETL process to capture and store the source data, so that the general ETL would follow the following pattern:

1. Run the snapshots command to capture the source data – `dbt snapshot`.
2. Run the models to read the data out of the snapshots and calculate the desired transformations – `dbt run`.
3. Run the tests to verify the results – `dbt test`.

A recent version of dbt has introduced the `dbt build` command, which can replace the three preceding commands, as it will run in the correct order all the commands to build all kinds of models, including seeds and snapshots, and the tests defined on them.

Running a snapshot will cause the following:

- The creation of a snapshot table from the result of the SELECT statement, which happens the first time that you run the snapshot
- The snapshot table to be updated, with the changes coming from the SELECT statement relating to the contents of the snapshot table, which happens any other time that you run the snapshot after the first

### Coding the SELECT query in a snapshot

In a snapshot, you can opt to just read the source unchanged and apply the changes we have seen in the STG model after the snapshot, but that will mean that every transformation has to be rerun every time that you read the data.

We prefer to put the transformation from the STG model in the snapshot so that we can run the transformations only once, and store the data transformed into the right data types, including the extra data that we have calculated (such as dates extracted from file names).
