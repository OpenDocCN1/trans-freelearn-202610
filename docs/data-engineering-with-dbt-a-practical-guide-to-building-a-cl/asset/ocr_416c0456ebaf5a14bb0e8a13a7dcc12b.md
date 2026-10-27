We will go with the fix path instead, as this is the path you will have to take often when you can’t really choose because you cannot affect the issue at the source.

Our source is a table with the data already in the date data type, so we cannot fix the parsing of the CSV; our fix will entail bringing dates from the 1st century to the 21st century.

As we are ingesting the CSV as a seed, we could also force the column to be of text data type and then parse it from text to date providing the correct format. That could be implemented as a macro in a similar way to the one shown here. You could take that as an exercise.

We could just write the SQL directly in the STG model, but to make it reusable, we will create a macro with the SQL and a parameter and show you how to use it in the STG model.

Let’s do it:

+   1. Create a `to_21st_century_date.sql` file in the macros folder with the following code:

```
```sql
{% macro to_21st_century_date(date_column_name) -%}
    CASE
        WHEN {{ date_column_name }} >= '0100-01-01'::date
        THEN {{ date_column_name }}
        ELSE DATEADD(year, 2000, {{ date_column_name }})
    END
{%- endmacro %}
```
```

In the preceding code, we have created the `to_21st_century_date` macro with a single `date_column_name` parameter. The code is pure SQL, with just the column name replaced by the parameter that will hold the column name.

Please note that the macro will be run in dbt to create the SQL to be sent to the database; therefore, the value of the parameter that we pass in is just a string with the column name, not the column values.

+   2. Open the `STG_ABC_BANK_POSITION.sql` file and edit the `src_data` CTE, editing the row for the `REPORT_DATE` column to look like this:

```
```sql
src_data as (
    SELECT
        ACCOUNTID   as ACCOUNT_CODE    -- TEXT
      , SYMBOL      as SECURITY_CODE   -- TEXT
      , DESCRIPTION as SECURITY_NAME   -- TEXT
      , EXCHANGE    as EXCHANGE_CODE   -- TEXT
      , {{to_21st_century_date('REPORT_DATE')}}
            as REPORT_DATE            -- DATE
      , QUANTITY    as QUANTITY        -- NUMBER
      , COST_BASE   as COST_BASE       -- NUMBER
      , POSITION_VALUE as POSITION_VALUE
```
```