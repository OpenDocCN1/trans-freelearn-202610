We can also use time travel to rapidly query the increase in the number of users in the last 30 days:

```
SELECT count(distinct user_id) -
  SELECT count(distinct user_id)
  FROM targe_table TIMESTAMP AS OF date_sub(current_date(), 30)
```

Another example would be to override a particular subset of data. First, we must load the specific version that we want to use:

```
df = spark.read.format("delta").option("versionAsOf", version).load("/mnt/delta/data")
```

Then, we can select a particular date range, which in our case is the first 15 days of June to be overridden by the ones in the previous version:

```
df.write.format("delta").mode("overwrite")\
  .option("replaceWhere", "insert_date >= '2020-06-15' AND insert_date <= '2020-06-15'")\
  .save("/mnt/delta/data")
```

You can also state why the changes in the data have been made, which you can do by storing them as metadata in the commits that are produced when these changes take place. This metadata can be later accessed in the operations history.

In the following example, we're replacing the complete version and state that we have made these changes to because the data was faulty:

```
df.write.format("delta").mode("overwrite") \
  .option("userMetadata", "updating-faulty-data").save("/mnt/delta/data")
```

The "overwrite" mode will replace the delta table stored in "/mnt/delta/data". It will specify user metadata with the userMetadata option to describe why the change was made, in the same way as a Git commit message.

The same can be done in SQL by setting the spark.databricks.delta.commitInfo.userMetadata option to the commit string:

```
SET spark.databricks.delta.commitInfo.userMetadata=updating-faulty-data
INSERT OVERWRITE data SELECT * FROM correctData
```