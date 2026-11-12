# # Relational databases

We'll also see an example of data ingestion from relational databases.

The following databases can be used to ingest data in Apache Spark:

+   * MySQL
+   * PostgreSQL
+   * Oracle
+   * SQL Server

To read data from a PostgreSQL database, you can try this code snippet:

```
from pyspark.sql import SparkSession
# Initialize a SparkSession
spark = SparkSession.builder \
    .appName("Read Data from PostgreSQL") \
    .getOrCreate()

# PostgreSQL database details
jdbc_url = "jdbc:postgresql://hostname:port/database_name"
properties = {
    "user": "username",
    "password": "password",
    "driver": "org.postgresql.Driver"
}

# Name of the table you want to read
table_name = "your_table"

# Read data from PostgreSQL into a DataFrame
df = spark.read.jdbc(
    url=jdbc_url, table=table_name, properties=properties)

# Show the DataFrame
df.show()

# Stop the SparkSession
spark.stop()
```