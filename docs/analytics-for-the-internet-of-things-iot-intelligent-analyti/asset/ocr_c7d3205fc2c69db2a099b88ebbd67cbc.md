The recommended row group size is 512 MB to 1 GB with a larger corresponding HDFS file block size of the same setting. This allows an entire row group to be read with one file block retrieval. The Parquet file extension is .parq.

# Avro

Avro is an Apache open source data serialization system project. The system processes data into a resulting container file that has some useful properties. It uses binary data format to keep the file size small and compact. This also results in faster read times.

![](bbox=[0.349, 0.307, 0.651, 0.386])

The structure of the data is stored in the file container. Rich data structures are supported. Avro file structures, or schemas, are defined using JSON. This is a schema-on-write process as far as file creation is concerned. This has the benefit of client applications not having to generate code to define the data structure. This makes using Avro files for analytics purposes much simpler since you do not need to discover and define the file schema in your analytics code.

There are Python and Java libraries that can be used to create and read Avro files. Most likely, you will use something such as Hive (discussed later in the chapters) to store data into Avro files. Avro supports primitive data types of null, boolean, int, long, float, double, bytes, and string. It also supports complex data types such as arrays, record, and enums. The following is an example JSON schema definition:

```
```json
{
  "namespace": "iotexample.avro",
  "type": "record",
  "name": "Sensor",
  "fields": [
    {"name": "sensor_id", "type": "string"},
    {"name": "temperature", "type": ["double", "null"]},
    {"name": "recorded_time", "type": ["string", "null"]}
  ]
}
```
```

Data records are converted to binary during serialization and can also be compressed to reduce size further. The file extension for Avro files is .avsc.