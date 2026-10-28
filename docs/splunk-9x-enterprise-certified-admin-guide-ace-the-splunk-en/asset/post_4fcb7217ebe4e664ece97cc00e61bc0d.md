
## Understanding Splunk indexes

An index is a specific type of data storage inside Splunk Enterprise; in other words, to keep it simple, an index is a repository of data. For example, you are searching for your address on a form online and you input your address details: you provide your house number, your street, and your postcode, which is unique to you. Similarly, if you want to search for specific data in Splunk, you can find it by using its index.

There are two types of Splunk indexes. They are called **event** indexes and **metrics** indexes. Event indexes store any type of text data, and this is the default index type. Metrics indexes only store metrics data, which must comply with a defined structure. There are special commands in Splunk, usually prefixed with m such as `mstats`, `mpreview`, and `mcatalog`, for working with metrics data. Metrics indexes are a completely different topic and beyond the scope of this book. If you would like to read more about them, please visit https://tinyurl.com/5n8jb6c. In this chapter, we will focus on event indexes. From now on, if there is no explicit description of the index type, assume it is an event index.

Splunk allows you to create your own indexes, and it also includes some event indexes, each serving a specific purpose. Let’s look at these indexes:

- `main`: The default Splunk index if one isn’t specified in external input sources.
- `summary`: A default summary index for storing summary data.
- `_internal`: Stores Splunk internal logs from a range of internal sources, such as splunkd daemon logs, web access logs, scheduler logs, python logs and more.
- `_audit`: Stores Splunk internal audit logs such as logins, searches, data accesses, and administrative activities.
- `_introspection`: Stores Splunk resource consumption and performance data. Monitoring console app dashboards highly depend on this index.
- `_fishbucket`: Stores checkpoint information of file-monitoring inputs.
- `_telemetry`: Stores instrumentation information if enabled through `telemetry.conf`.

There is more than one way to create a Splunk index. It all depends on the type of Splunk deployment architecture. Regardless of the approach, you do the configuration of the `indexes.conf` file in the indexer.

The indexer is a Splunk Enterprise instance that processes incoming data before storing it in indexes and responds to search queries issued by a search head. We had a brief overview of Splunk components such as the indexer and search heads in previous chapters.

Let’s go through the important characteristics of indexes:

- Splunk allows administrators to create custom indexes to store and organize specific types of data based on their needs and requirements.
- Every index has data size and retention time limitations. To view the configuration settings, see the indexes.conf definition of _internal index in the $SPLUNK_HOME/etc/system/default directory.
- Indexes contain originally received data in a compressed format, which is called raw data. They also contain index files called tsidx files and metadata files.
- Indexes organize data in the form of buckets on the filesystem.
- Indexes are by default stored at $SPLUNK_HOME/var/lib/splunk. The path is referred to with the $SPLUNK_DB environment variable.
- Once the data is written to an index, it is final and cannot be modified.
- An authorized user that has the can_delete role can issue a delete command to prevent data from participating in a search. This command applies delete markers to data. This means it doesn’t permanently remove the data from storage. To permanently delete event data, execute the splunk clean command with caution.
- The segregation of indexes depends on data size, data retention, access policies, and the type of data being stored in them.

I hope you now have enough of an understanding of indexes. Let’s go through how Splunk organizes data in the form of buckets in the next section.

## Understanding buckets

Buckets are an integral part of indexes; they contain raw data and index files. They are organized in the form of folders on a filesystem with a specific naming pattern. These folders are explicitly used by the Splunk indexer for data storage and search processing.

In order to learn a bit more about them, let’s look at the default _introspection index folder structure:

```
C:\Program Files\Splunk\var\lib\splunk\_introspection>tree
Folder PATH listing for volume Windows
Volume serial number is 6676-C13F
C:.
├── colddb
├── datamodel_summary
├── db
│   ├── db_1663851317_1661686760_13
│   │   └── rawdata
│   ├── GlobalMetaData
│   └── hot_v1_14
│       └── rawdata
└── thaweddb
```

Figure 5.1: Splunk non-clustered index folder structure

We’ve now looked at event index buckets. Data gets rolled from hot to warm to cold to frozen to thawed buckets depending on the situation. Now that you are familiar with indexes and their internal structure, let’s move on to the creation of indexes.

## Creating Splunk indexes

Creating a custom index is a Splunk administrator’s responsibility. As you have seen, data is retained in indexes until it reaches a certain age or the index reaches a certain size, so an estimation of an index’s size before it is created is a crucial step. Having an accurate estimation retains the data for the necessary amount of time, and it also helps to determine the required storage capacity upfront.

In order to estimate the size of the index, we must have the answers to the following questions:

- *Question:* How long does the data need to be retained in days?
  *Answer:* For example, 100 days
- *Question:* How much data volume per day is expected in GB?
  *Answer:* For example, 1 GB

The formula for index size estimation is as follows:

*Retain days X volume per day X 1/2 (raw data compression + index files)*

If we substitute in the example values, the index size becomes 100 X 1 X ½ = 50 GB.

Now that you know how to estimate the size of an index and have decided on a name for it, let’s go through the index creation process.

In the *Understanding buckets* section, we saw that the `indexes.conf` file in the `$SPLUNK_HOME/etc/system/default` directory has an entry for the `_introspection` index. Similarly, Splunk ships with many internal indexes that you can find in the same file. Splunk allows us to create custom indexes, and there is more than one way to create an index. It all depends on the type of deployment architecture. In general, the administrator chooses the most suitable approach. In this section, we have chosen a single instance of Splunk, which serves as both the search head and the indexer. Let’s take a look at the different ways to create event indexes.

## Splunk Web

Log in to Splunk Web as an administrator. Go to the **Settings** menu in the top right and click on **Indexes**. A page opens with a **New Index** button in the top right. On the same page, you can see the default indexes are prefixed with _, such as `_internal` and `_audit`.

If you wish to see the configuration for one of the default indexes, click on the index name. Otherwise, proceed with creating a new index by clicking the **New Index** button and filling in the **Index Name** and **Max Size of Entire Index** fields. The default **Index Data Type** setting is **Events**. Metrics should

- **Parameters:** homePath, coldPath, thawedPath, app, and so on
- **Example:** $SPLUNK_HOME\bin\splunk add index windows-event-logs -app search

Both the CLI and Splunk Web approaches write to the indexes.conf file on the filesystem. If you are familiar with the indexes.conf specification, then creating an indexes.conf file directly achieves the same purpose.

## indexes.conf explained

Let’s deep dive into the indexes.conf file for the windows-event-logs index that we have created through Splunk Web. As we learned in the Splunk Web section, the index has by default been saved to the Search & Reporting app folder called search, hence I can find it at $SPLUNK_HOME/etc/apps/search/local:

```
[windows-event-logs]
coldPath = $SPLUNK_DB\windows-event-logs\colddb
enableDataIntegrityControl = 0
enableTsidxReduction = 0
homePath = $SPLUNK_DB\windows-event-logs\db
maxTotalDataSizeMB = 500
thawedPath = $SPLUNK_DB\windows-event-logs\thaweddb
```

One of the advantages of this approach is there is a range of options that can be set for advanced index configurations. However, the creation of a new index through this file-based approach requires the Splunk instance to be restarted.

There are ways to control the size of data from the bucket directory level to the total index size. Let’s take a look at the following advanced options and what to back up:

- homePath.maxDataSizeMB limits the homePath directory size for hot and warm buckets.
- coldPath.maxDataSizeMB limits the coldPath directory size for cold buckets.
- To limit the number of hot and warm buckets that can be created, set maxHotBuckets and maxWarmDBCount, respectively. For high-volume indexes, where the indexing size is more than 10 GB/day, Splunk suggests increasing maxHotBuckets from the default of 3 to 10.
- By default, Splunk limits the hot bucket size to 750 MB through maxDataSize = auto, and if the index size per day estimation is 10 GB, set this option to maxDataSize = auto_high_volume for Splunk to create 10 GB hot buckets on 64-bit systems. The reason for having this option is to avoid creating too many small buckets for large indexes. Having too many small buckets affects Splunk’s performance. Hot buckets will roll to warm buckets once they reach their maximum size and then a new hot bucket will be created.
