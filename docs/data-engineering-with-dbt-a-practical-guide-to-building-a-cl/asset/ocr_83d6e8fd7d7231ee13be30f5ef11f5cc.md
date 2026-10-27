Then, we have a config line to make the model incremental:

```
```jinja
{{ config(materialized='incremental') }}
```
```

Please note that by not providing a unique_key parameter to the config, we instruct dbt to append all the rows returned by our SQL code to the incremental model.

The macro’s design is based on the dbt is_incremental() macro to run the right code depending if we already have the underlying table or if we are creating it.

The macro returns true if the incremental table already exists and the run of the model is a normal run for an incremental model; it returns false when the target table does not exist (yet) or when the run is a full-refresh run. This can be achieved by passing the --full-refresh argument to recreate the table from scratch.

```
```sql
WITH
{%- if is_incremental() %}
```
```

If we already have the target table, for each key, we need to compare only the latest version stored in the HIST table with the incoming data, to see what is unchanged and does not need to be saved and what is new or changed and needs to be saved.

```
```sql
current_from_history as (
  -- get the current HDIFF for each HKEY
),
```
```

By ensuring that the HDIFF column contains the source payload, including the business key of the entity, and excluding only the metadata (and any other field that we do not want to track changes on), it acts like a unique content identifier.

Therefore, we can do an equijoin on the HDIFF column alone, to achieve the most efficient comparison in a columnar database.

```
```sql
load_from_input as (
  -- join the input with the HIST on the HDIFF columns
  -- and keep the rows where there is no match
)
```
```

When the HDIFF from the input and the HIST are the same, it means that we already have that version of the data stored as the most recent version. Then, we need to load from the input only the rows where the HKEY does not match between the input and HIST table. These are the rows where the input is different from the current or new.

A crucial step of this load pattern is to be sure that, in both the input and the current_from_history, we only have one row for each key. If we have multiple rows, one HDIFF can never match two different ones, so the load will be incorrect. The current_from_history macro satisfies this requirement by design, so you need to ensure that the input is unique by HKEY.