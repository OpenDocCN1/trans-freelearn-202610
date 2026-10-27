The preceding snippet exemplifies the field declarations for the fields defined in the `field_expressions` variable example in this section.

Please be aware that by storing $1 in the source data in variant form, you are duplicating the size of the data that you are storing, so it only makes sense in the first development phase if you are having trouble with extracting data from the variant or if you have decided to extract very few fields and keep most of the data stored as variants as your main storage.

Note that this can be a sensible solution when unpacking and flattening the schema of the semi-structured entity is either very complex or will bring problems with the object-relational conversion.

One typical example is when unpacking documents with headers and rows. This is a common example of **nested hierarchical data** stored in semi-structured files.

If the rows do not have a stable, unique identifier and are just identified by the header ID plus the row number, which is often done with orders or invoices, then it is impossible to track the rows by themselves (think of what to do if a row gets deleted, with no changes in the other rows), so it is better to track the versions of the document as a single thing and then unpack it in the ref layer.

One alternative is to flatten headers and rows but create a HDIFF of the rows so that if any changes occur (in the header or rows), all existing rows will be saved with the same LOAD_TS as the changed header and can all be retrieved using a QUALIFY clause with a rank function partitioned on the HKEY header and sorted on decreasing LOAD_TS. rank=1 will retrieve all the rows written at the last LOAD_TS.

Now that we have seen the loading macro for the semi-structured data, let’s have a quick look at the macro that implements the loading pattern – the `ingest_semi_structured_into_landing_sql` macro:

```
```sql
{% macro ingest_semi_structured_into_landing_sql(
        full_table_name, field_expressions,
        file_pattern, full_stage_name, full_format_name
) %}
BEGIN TRANSACTION;
COPY INTO {{ full_table_name }}
FROM (
    SELECT
        {{ field_expressions }},

        METADATA$FILENAME as FROM_FILE,
        METADATA$FILE_ROW_NUMBER as FILE_ROW_NUMBER,
        '{{ run_started_at }}' as INGESTION_TS_UTC
    FROM @{{ full_stage_name }}/
)
```
```