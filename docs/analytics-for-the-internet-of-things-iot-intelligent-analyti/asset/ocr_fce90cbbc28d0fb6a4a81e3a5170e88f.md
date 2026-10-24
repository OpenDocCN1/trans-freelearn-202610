# The retention strategy example

We will return to the GPS position data example to set up a retention policy. The raw data is landed into an HDFS table and kept uncompressed. A series of data refineries transforms the data into cleaned and useful analytical datasets. All of the analytical datasets are compressed in Parquet format—still in HDFS.

Upon the review of the raw data, it was determined that any problems in the initial transformation are typically discovered within a week. No problems were found more than a month later. Due to this, the raw data was retained for the latest month, and then moved into S3 Standard. In S3, a ruleset was created to move the data into Standard-Infrequent Access after another month, and then into Amazon Glacier a month after it. After another three months, the data is scheduled to be deleted from Amazon Glacier.

After several months of statistical analysis and modeling, several key fields were identified in each analytical dataset. It was also discovered that data records from more than two years prior were rarely accessed. Records from more than three years prior were never accessed.

The data from two years prior was moved out of HDFS into S3 Standard. Data between two and three years old was transformed to only keep the key fields. The older data is moved to Amazon Glacier. After four years, the data is summarized at a daily aggregation with metrics on record counts, starting, ending, and average position for each device along with distance traveled and the travel time. This data is kept in S3 Standard as it is 10,000 times smaller in size. The full data is then removed.

# Summary

In this chapter, we reviewed how to increase data scientist productivity by reducing time spent manipulating data. The concept of LAD was introduced, along with a process to build them.

Concepts to help prevent data lakes from turning into a data swamp was discussed. Changing perspectives from a data lake to a series of active refineries and production chains helps to plan for a useful data storage area.

Strategies for data retention were also reviewed. Consider the twin goals of maximizing value while minimizing costs when designing a retention strategy. In the next chapter, we will discuss the economics of IoT analytics. The chapter covers ways to look for business value in the data along with a detailed predictive maintenance example.