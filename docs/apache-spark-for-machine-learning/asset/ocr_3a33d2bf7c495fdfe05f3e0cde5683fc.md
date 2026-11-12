# NoSQL databases

Let's review an example of data ingestion from NoSQL databases.

The following NoSQL databases can be employed for data ingestion:

+   - Cassandra
+   - HBase
+   - MongoDB

Here is a sample code snippet to read data from a Cassandra NoSQL database:

```
from pyspark.sql import SparkSession

# Configure the Spark session to use the Spark Cassandra Connector
# Replace with the correct version
connector_package = "com.datastax.spark:spark-cassandra-connector_2.12:3.0.0"

spark = SparkSession.builder \\
    .appName("PySpark Cassandra Example") \\
    .config("spark.jars.packages", connector_package) \\
    .config("spark.cassandra.connection.host", "cassandra_host") \\
    .config("spark.cassandra.connection.port", "cassandra_port") \\
    .config("spark.cassandra.auth.username", "username") \\
    .config("spark.cassandra.auth.password", "password") \\
    .getOrCreate()

# Now you can use the Spark session to read from or write to Cassandra
# Example: Reading data from a Cassandra table
keyspace = "your_keyspace"
table = "your_table"

df = spark.read \\
    .format("org.apache.spark.sql.cassandra") \\
    .options(keyspace=keyspace, table=table) \\
    .load()

df.show()

# Remember to stop the Spark session
spark.stop()
```