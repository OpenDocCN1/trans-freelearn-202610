In addition to the Spark framework, Glue ETL provides Glue-native advanced frameworks/APIs and formats for complex ETL, such as GlueContext, DynamicFrame, job bookmarking, Glue Parquet, Glue Interactive session, workflows, triggers, and more.

Let’s explore these features one by one.

# AWS GlueContext

The `GlueContext` class wraps Apache’s `SparkContext` and can be initialized using the following code snippet:

```
```python
sc = SparkContext()
glueContext = GlueContext(sc)
```
```

GlueContext allows you to communicate with the Spark framework and is the entry point for many Glue ETL features, such as DynamicFrame, job bookmarking, job parameters, and more.

# DynamicFrame

Glue ETL’s DynamicFrame is a vital feature that enables Spark developers to create more efficient Spark pipelines, and it is native to Glue. A Glue DynamicFrame is a distributed collection of self-describing DynamicRecord objects, but it does not require them to adhere to a fixed schema. Due to the self-describing nature of records in DynamicFrame, it does not require a schema to be created and can be used to read/transform data with inconsistent schemas.

You can create a Glue DynamicFrame using different APIs, depending on your use case. For example, the following code snippet creates a DynamicFrame object called `datasource0` for the `my_table` table in the `my_database` database:

```
```python
datasource0 = glueContext.create_dynamic_frame.from_catalog(database_
name='my_database', table_name='my_table', transformation_
ctx='datasource0')
```
```

In this statement, we used the GlueContext object, which uses the Glue SDK, to connect to Glue Data Catalog and fetch the data source classifications and properties to create the object. Users can also pass additional options using the `additional_options` parameter, such as the `push_down_predicate` parameter.

Besides using the `from_catalog` method to create a DynamicFrame, you can create DynamicFrames using the following methods:

+   - **`from_options`**: This method allows users to create DynamicFrames by manually specifying the connection type, options, and format. This method provides users with the flexibility to customize options for a data store.
+   
+   - **`from_rdd`**: This method allows users to create DynamicFrames using Spark RDDS.