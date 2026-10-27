
The macro declaration is pretty much the same as the normal save_history macro, with the addition of the del_rel and del_key_column parameters, which contain the relationship to the model with the list of deleted entities and the column with the key for the input entities, respectively:

```
{% macro save_history_with_deletion_from_list(
    input_rel,
    key_column,
    diff_column,

    del_rel,
    del_key_column,

    load_ts_column = 'LOAD_TS_UTC',
    input_filter_expr = 'true',
    high_watermark_column = None,
    high_watermark_test = '>=',
    order_by_expr = None
) -%}
```

Here, or in the model calling the macro, we need to have the incremental model configuration:

```
{{ config(materialized='incremental') }}
```

As we often do with incremental models, we start with the code for the incremental case.

First, we read the current data from the history table as usual:

```
WITH
{% if is_incremental() %}  -- in an incremental run
current_from_history as (
    {{- current_from_history(
        history_rel = this,
        key_column = key_column,
        load_ts_column = load_ts_column,
    ) }}
),
```

Next, we must keep the rows from the input that contain a new entity or a new row compared to the history. This is very similar to the normal save_history macro.

Note that now we filter the history to compare only against active entities.

You should either put tests in place that state you cannot have the same keys in both sources or handle that case in the desired way in the two STG models that provide the input and deletion list to the history table.

The macro has the following clear, but not idempotent, behavior:

- When an entity is marked as deleted in the `HIST` table, it will be added again if it’s present in the input.

- When an entity is not marked as deleted in the `HIST` table, it will be marked as deleted if it’s present in the `DEL` input.

    If the version in the input is different from the version in the history, then both the new version and the deletion will be added to the `HIST` table, breaking the contract of the history table to have one entry for each load timestamp.

    This could create problems and duplicates as two current versions would exist, but because one version would be marked as deleted, the most probable behavior is that the deletion would be practically ignored.

- The combined result of these two outputs is that if a key is present in both the input and the deletion list, the result will depend on its state and version in `HIST`: if the key has been deleted, it will be undeleted, while if it hasn’t been deleted, the results will depend on the versions being equal or not.

As an example, to remove this, you can prioritize one of the inputs by putting one of the following pieces of logic in the STG models that feed the history table:

- To give priority to the input, the keys that appear in the input must be removed from the deletion list

- To give priority to the deletion list, the keys that appear in the deletion list must be removed from the input

With this final discussion, we have provided you with some tools to get the most out of this powerful pattern, which operates in a complex situation and needs to be understood well by the analytics engineer designing the models.

## Storing history with multiple versions in the input

So far, we have only been adding one version at a time to the history – the “latest” from the input – so it is natural that it is either the same version that is already in the history or some more recent version.

In this section, we will look at a pattern you can use to load multiple versions in a single load round, making it possible to load data at any velocity and keep accurate audits for storing all the versions that can be observed in our input (the STG model on top of the source).

The pattern that we propose here uses the current version in the history as the baseline, comparing the current from history with the first (oldest) of the inputs for each instance (key) and then the remaining inputs in their order from the input.

This demands having a few easy-to-achieve but important conditions:

- The order of the versions must be stable. You need to provide a sorting expression to ensure that the versions are evaluated in the right order.

    The most common situation is to have some effectivity or last modified timestamp that provides the version order, but anything that produces a sort without multiple rows with the same rank will work, including sorting on the ingestion filename and row number.

    After all, if you cannot establish an order for the versions that you receive, how can you tell which one is the current/most recent? This might be a sign that you are missing something.

- To have a lean, well-ordered sequence of versions, the input must only contain versions that are aligned or more recent than the current version from the history.

    If your input contains versions older than the current from the history, the history will contain the full sequence of versions in the right order, but the versions before the current one from the history will be inserted again without any need, generating rows that are duplicates except for the load timestamp.

    The natural solution is to use a high watermark column to filter the data incoming from the STG model based on the content of the history table.

    This pattern is well suited for ingesting delta loads but can work equally well with full exports, so long as you can have a high watermark that is part of the business information, such as a last modified or an effective timestamp, and not some loading metadata such as an ingestion timestamp.

    In this pattern, the high watermark is implemented at the table level, implemented as max of the given column. It is a common situation and provides good performance, but the pattern could be modified to have a per-key level.

- Another important consideration is late arrivals.

    Data is added to the history table considering the arrival timeline, so input data is considered arrived at the same time or after the previous load.

    Versions for each key will be compared with the “previous” version based on the provided sorting expression. The first input is compared to the current from history; the others are compared to their predecessor in the input.

    In many cases, it is possible to architect the HDIFF column, the sort expression, and the load timeline to store data arriving late with the acceptance that some data might be stored twice, but you can read data out of the history table correctly.

    If you have multiple timelines and for your use case, it is not good enough to compare the input with the most current record in the history table based on one timeline (load timestamp or another), this pattern is not suitable for you.

So far, we have been ingesting the latest version from the input. Now, we can save all the new information that arrived after the last ingestion in one go if we can identify this starting point. Usually, that is done with the last updated column.

Before we jump into the code, we can already say that the ingestion mechanism is very similar to the other macros to save history. What makes this different is the ability to store multiple versions of an instance for a single load timestamp, and conversely, the ability to get the single-most current instance from the history despite having multiple versions with the same load timestamp.

As we mentioned previously, to achieve this we need a sorting expression to put the multiple versions of one instance in order.

To implement this pattern, we will implement two macros: one is an extension of current_from_history and is for selecting the last row from the multiple rows from the last load time; the other is an extension of save_history, which applies the correct comparison based on the order of the versions.

Let's start by discussing the save_history_with_multiple_versions macro.

You will immediately notice that the declaration here is the same as the one in the save_history macro, except for the addition of the sort_expr parameter, which is new and mandatory.

This new parameter needs to provide a SQL expression that, when combined with ORDER BY, produces stable sorting for the input, with the oldest version first and the newest last.

As an example, LAST_UPDATED asc or FILE_NAME, ROW_NUMBER would be two good expressions, the first working in all cases, including a full export, and the second suitable for incremental loads:

```
{% macro save_history_with_multiple_versions(
    input_rel,
    key_column,
    diff_column,
    sort_expr,
    history_rel = this,
    load_ts_column = 'LOAD_TS_UTC',
    input_filter_expr = 'true',
    history_filter_expr = 'true',
    high_watermark_column = None,
    high_watermark_test = '>='
) -%}
```

Then, we have the config to declare the incremental materialization:

```
{{- config(materialized='incremental') }}
```

We include the config in the macro as the macro makes no sense without incremental materialization, but you can take it out and have it in your models.
