# # HDFS I/O

An HDFS read operation from a client involves the following:

+   1. The client requests NameNode to determine where the actual data blocks are stored for a given file.
+   2. NameNode obliges by providing the block IDs and locations of the hosts (DataNode) where the data can be found.
+   3. The client contacts DataNode with the respective block IDs to fetch the data from DataNode while preserving the order of the block files.

![](bbox=[0.22, 0.382, 0.782, 0.626])

An HDFS write operation from a client involves the following:

+   1. The client contacts NameNode to update the namespace with the filename and verify the necessary permissions.
+   2. If the file exists, then NameNode throws an error; otherwise, it returns the client `FSDataOutputStream` which points to the data queue.
+   3. The data queue negotiates with the NameNode to allocate new blocks on suitable DataNodes.
+   4. The data is then copied to that DataNode, and, as per the replication strategy, the data is further copied from that DataNode to the rest of the DataNodes.