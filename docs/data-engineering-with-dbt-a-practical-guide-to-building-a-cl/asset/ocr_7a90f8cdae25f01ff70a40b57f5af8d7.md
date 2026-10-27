```
INGESTION_TS_UTC TIMESTAMP_NTZ(9)
)
COMMENT = '...';
{%- endmacro %}
```

As you can see, there is only so much to say here as this macro just encapsulates a normal CREATE TABLE SQL command.

The interesting point is that you should declare all the columns that are present in the CSV in the initial part of the table, leaving the three metadata fields at the end, as they are created automatically by the ingestion macro in the last three positions.

As a note, you can define the nullability of the fields, with the default being nullable. Unless you want to have a very strict and inflexible loading setup, we suggest leaving all columns as nullable and checking the ones that you want to be not null with tests on the source that you will define to read from the landing table.

Finally, let’s look into the ingest_into_landing_sql library macro, which implements the call of the COPY INTO command:

```
{% macro ingest_into_landing_sql(
    full_table_name,
    field_count,
    file_pattern,
    full_stage_name,
    full_format_name
) %}
BEGIN TRANSACTION;
COPY INTO {{ full_table_name }}
FROM (
    SELECT
    {% for i in range(1, field_count+1) %}${{i}},{% endfor %}

    METADATA$FILENAME as FROM_FILE,
    METADATA$FILE_ROW_NUMBER as FILE_ROW_NUMBER,
    '{{ run_started_at }}' as INGESTION_TS_UTC
    FROM @{{ full_stage_name }}/
)
PATTERN = '{{ file_pattern }}'
{%- if full_format_name %}
FILE_FORMAT = (FORMAT_NAME = '{{ full_format_name }}')
{%- endif %}
;
```