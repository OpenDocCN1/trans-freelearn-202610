Unless you have special security needs, we suggest creating one storage integration for each bucket/storage account, dividing it with top folders/containers by source system, and then pairing the stages with the top folders/containers that give access to the files for the tables of one specific system. Under the system level folder a folder for each table or topic or message type from that system is a good initial organization.

If you have some tables that require special security, we suggest hosting their data in a different bucket/account and creating separate integrations and stages for each group of people that can access such data. This allows you to provide their roles with the usage of the integration (if they have to manage the stages) or of the stages to read the files.

Creating a storage integration is a different process for each cloud provider, so we suggest that you look up the process for your cloud provider on the page for the CREATE STORAGE INTEGRATION command in the Snowflake documentation at https://docs.snowflake.com/en.

# How to parse your files

Once we know where the files are and we are authorized to read them, we still have to tell Snowflake how to read them.

For many of the semi-structured formats such as Parquet, AVRO, and ORC, there are just a few options as the formats both have schema descriptions of the content and a clear way to read and write the content.

For the CSV and, to a minor level, JSON formats, we have a bit more variability, and when creating a format, it is important to provide the needed options so that Snowflake can correctly read and represent the data.

For each file type supported by Snowflake, there is a default file format implicitly defined by the defaults of the CREATE_FILE_FORMAT command. So, to define a format, you only need to provide the values for the options that do not align with the default value.

Options that often need to be specified are the number of rows to skip as a header, the field delimiter, the file encoding, the escape character, and the date and time formats.

Here is an example of a file format definition with some basic options defined:

```
CREATE FILE FORMAT IF NOT EXISTS file_format_name
  TYPE = 'csv'
  FIELD_DELIMITER = '|'                -- default ','
  SKIP_HEADER = 1                      -- default 0
  ENCODING = 'ISO-8859-1'              -- default 'UTF8'
  FIELD_OPTIONALLY_ENCLOSED_BY = '"'  -- default NONE
;
```