# Configuring the Delta cache

It is recommended to always use Delta cache-accelerated clusters for worker instances, which are optimized instances running SSD disks. These instances are already configured to work with the Delta cache as default, but you can manually configure its parameters using Apache Spark options during the session creation:

+   - To establish a maximum disk usage, which is set by default to half of the worker local storage, you can use the maxDiskUsage option, measured in bytes, as shown in the following example, where we set it to 50g:
+   
+     ```
+     spark.databricks.io.cache.maxDiskUsage 50g
+     ```
+   - We can also specify the maximum allocated space measured in bytes for metadata with the maxMetaDataCache option, as shown in the following example, where it is set to 1g:
+   
+     ```
+     spark.databricks.io.cache.maxMetaDataCache 1g
+     ```
+   - We can also disable the compression, which is enabled by default, as follows:
+   
+     ```
+     spark.databricks.io.cache.compression.enabled false
+     ```
+   - To disable the Delta cache, we can use the following Scala command:
+   
+     ```
+     spark.conf.set("spark.databricks.io.cache.enabled",
+     "false")
+     ```

It is good to know that when disabling the Delta cache we will not lose data that was already stored in the worker local storage, but we will avoid more data being added.

In the next section, you will read about DFP, which is a Delta Lake feature that improves query performance.

# Optimizing queries using DFP

DFP is a Delta Lake feature that automatically skips files that are not relevant to a query. It is a default option in Azure Databricks and works by collecting data about files in Delta Lake, without the need to explicitly state that a file should be skipped on a query, improving performance by making use of the granularity of the data.