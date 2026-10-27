
The preceding code should be run on the STG model after you have implemented the src_data CTE to produce quite stable column names, data types, and order of the fields for the model.

The following screenshot shows the result of running this query on the STG model for the ABC Bank position entity:

| Row | SQL_TEXT |
| --- | --- |
| 1 | , '-1' as ACCOUNT_CODE |
| 2 | , '-1' as SECURITY_CODE |
| 3 | , 'Missing' as SECURITY_NAME |
| 4 | , '-1' as EXCHANGE_CODE |
| 5 | , '2000-01-01' as REPORT_DATE |
| 6 | , -1 as QUANTITY |
| 7 | , -1 as COST_BASE |
| 8 | , -1 as POSITION_VALUE |
| 9 | , '-1' as CURRENCY_CODE |

Figure 6.3: Generated code for the default record

Also, in this case, you can take the generated code, add SELECT before the first row, remove the extra initial comma, and have a working definition for your default record that you can keep as is or manually improve upon.

## Adding metadata

Another common task is to record metadata that comes from the source system and add other metadata that we can generate.

Some of the “metadata” fields will come from normal fields of our source systems, while others will be added by the loading pipeline. By defining a metadata field, we copy the relevant information under the metadata name, preserving the original field, if it is useful also by itself. Because the same metadata can be called in many ways in different source systems, it is useful to always get the metadata into a field with the same name.

The following are some forms of metadata that might be added:

- LOAD_TS: This is the time when the data is loaded into our data platform. This is a must and does not depend on the source system. We usually add it as the last field.
- RECORD_SOURCE: This is the place the data comes from. This might be available from the data movement pipeline (for example, the filename of the CSV we are reading from) or not, so you can make up a description for it, such as the view/source table name. This is also a must, as it allows you to audit/understand where your data is coming from.
- EXTRACTION_TIME: This is the time when the data was extracted from the source system. Often, we do not read the source directly, so it might be quite different from LOAD_TS.
- CREATION_TIME: This is the time when the information was first created. Many systems record this information, and if this is generally available, it is useful to create such a metadata field.
- EFFECTIVE_FROM: This is the time when the information becomes effective for business purposes. This is purely a business field that allows you to handle multi-temporality – that is, multiple timelines. It is very important in all cases to distinguish between when you received some information and when you need to consider the information to be in use.

Of course, you can come up with your own metadata to describe any aspect of the source system and data platform processes as needed.

## Identity and change detection fields

The last thing that we add in the STG model is three fields that will help in the storage of the entity:

- HASH_KEY: This is a single field with all the columns that make up the primary key of the entity chained together. The result is usually hashed, hence the name HASH_KEY. Its primary use is to provide a primary key in a single field, to standardize and simplify the loading of the history. By defining the HASH_KEY in your code, you give a clear, explicit definition of what the primary key is for the entity.
- HASH_DIFF: This is a single field with all the fields that we want to test for changes chained together. The result is usually hashed, hence the name HASH_DIFF. This field is used to simplify and standardize the loading of the history and provides an explicit definition of what is considered a change. It usually contains all fields except for the metadata ones.
- LOAD_TS: This is a timestamp that provides a timeline of the changes of the entity as they are received and stored by the data platform. This is metadata that we add as part of the loading process.

Using the HASH_KEY field, we are able to pick all the versions of a specific entity, whereas by using LOAD_TS, we are able to know what version is current or was current at some specific moment in time.

The HASH_DIFF field is a purely technical field that can be considered a version identifier, but be aware that it is perfectly normal for an entity to go back to the same set of values it had at some point in the past. Therefore, it might very well be that we have two equal hash diff values separated in the history table by one or more different ones.

In some cases, we will add more than one HASH_KEY, as the entities that we are storing might be viewed in more than one way (for example, an account can be looked at as a local or as a global account).

At times we will also add hashed keys for composite foreign keys that we want to store in the history table. This can be a common situation if you build hierarchies or dirty dimensions that use a hash made of multiple fields as a key. It is anyway more common to add such calculated keys in the REF layer.

## Completing the STG model

Previously in this chapter, we have created an initial version of the STG model for the ABC Bank’s positions. Let’s complete it by adding the sections and following the pattern described in the previous section.

You can find the second version in full in the `06_02_v2__STG_ABC_BANK_POSITION.sql` file in the dbt folder, under the Chapter_06 materials of the book’s repository. Here, we will describe each section by itself.

Let’s start by moving the current version into the `src_data` CTE and adding some metadata:

```sql
WITH
src_data as (
        SELECT
                ACCOUNTID        as ACCOUNT_CODE    -- TEXT
        , SYMBOL               as SECURITY_CODE   -- TEXT
        , DESCRIPTION          as SECURITY_NAME   -- TEXT
        , EXCHANGE             as EXCHANGE_CODE   -- TEXT
        , REPORT_DATE          as REPORT_DATE     -- DATE
        , QUANTITY             as QUANTITY        -- NUMBER
        , COST_BASE            as COST_BASE       -- NUMBER
        , POSITION_VALUE       as POSITION_VALUE  -- NUMBER
        , CURRENCY             as CURRENCY_CODE   -- TEXT

        , 'SOURCE_DATA.ABC_BANK_POSITION' as RECORD_SOURCE

        FROM {{ source('abc_bank', 'ABC_BANK_POSITION') }}
),
```

The only real addition in the preceding code is the RECORD_SOURCE metadata.

We do not have any real data incoming to tell us where the data is coming from, so a common solution is to use a string with the name of the view/table that we are reading.

The next section would be the `default record` CTE, if this was a dimension, but as this is a fact – that is, a table listing events that happened or describing the state of a system, and not descriptive information about an entity – we do not need a default record.

As we do not have a default record, the `with_default_record` CTE is not needed.

The last part of the STG model is the hashed CTE, which you can see in the following code block. Currently, we will not have any hashing, as we have not yet introduced the feature that we will use for hashing. Chaining the fields will work well enough anyway:

```sql
hashed as (
    SELECT
        concat_ws('|', ACCOUNT_CODE, SECURITY_CODE) as POSITION_HKEY
      , concat_ws('|', ACCOUNT_CODE, SECURITY_CODE,
          SECURITY_NAME, EXCHANGE_CODE, REPORT_DATE,
          QUANTITY, COST_BASE, POSITION_VALUE, CURRENCY_CODE )
        as POSITION_HDIFF
      , *
      , '{{ run_started_at }}' as LOAD_TS_UTC
    FROM src_data
)
SELECT * FROM hashed
```

This part of the code does the following:

- Defines the composite primary key, `POSITION_HKEY`, as the concatenation (without hash for now) of the `ACCOUNT_CODE` and `SECURITY_CODE` fields.

  This is one of the potential choices; as always when picking keys, the important consideration is that they reflect the reality of the situation. In theory, we could have in one account the same security quoted on different markets, and the quote of the security depends on the market you are looking at.

  We will start with this definition and eventually will change it later, if needed. Changing a key is never easy, so it’s better to think well about it, but on the other hand, we should not get paralyzed by doubts and potential pros and cons. If you really have two good alternatives, find two names and define them in a consistent way, and then make extremely evident the one you use to store the history.

- Similarly, we have defined the change control field, `POSITION_HDIFF`, as the concatenation of all the non-metadata fields of our Position entity.

  Including all fields, key fields included, is a standard process that makes it possible to check with a single comparison, if even a huge record with tens or hundreds of fields has changed since a previous version.

- We have used the star symbol, `*`, to bring forward all the fields from the source data and the metadata that we have added in the previous CTEs.
