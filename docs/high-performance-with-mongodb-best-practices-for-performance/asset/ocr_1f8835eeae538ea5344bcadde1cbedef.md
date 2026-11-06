# 7

# Storage Engines

As we saw in Chapter 1, Systems and MongoDB Architecture, WiredTiger is a component within the MongoDB server and the most common storage engine for MongoDB. Storage engines act as the interface between the logical structure of the data (as seen by users, applications, and higher layers of the database) and the physical storage of that data. As such, they are a critical factor when diagnosing and resolving performance issues. This chapter aims to equip you with a high-level understanding of the WiredTiger storage engine, enabling you to make informed decisions about its configuration. By the end of this chapter, you will have a solid grasp of WiredTiger’s performance-related inner workings and the knowledge to fine-tune it for efficiency.

The topics covered in this chapter include the following:

+   - An overview of storage engines and their role in MongoDB
+   - The architecture of WiredTiger, including its storage format, concurrency control, and caching strategies
+   - How WiredTiger manages its cache, ensures data durability, and handles recovery from unexpected failures
+   - The compression algorithms supported by WiredTiger, and the trade-offs between compression efficiency and resource usage
+   - Key configuration options for WiredTiger and how to tune them for your specific workload and hardware setup