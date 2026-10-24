If a node fails and file blocks are lost, HDFS automatically generates another copy of the blocks from the remaining copies on other nodes to replace them. These are distributed across the other normally operating nodes. The NameNode keeps track of all these operations. File blocks are also moved around for performance optimization purposes. The actual duplication of file blocks is handled by the data nodes in communication with each other.

To get an idea of how this works, we will describe the high-level process of writing a file. A client, which could be a command-line prompt on the MasterNode or some other software that connects to the cluster, requests a write or read of a file. This request goes to the NameNode, which returns the list of node identifiers where the file blocks can be retrieved or written to. When a file is written, the file blocks are written once by the client request to the nodes in the list. Then, the data nodes duplicate it the set number of times by communicating among themselves.

HDFS file commands are very similar to Linux filesystem commands. The following example instructs HDFS to take the local file called lots_o_data.csv and distribute it across the HDFS cluster in the name space, /user/hadoop/datafolder:

```
hdfs dfs -put lots_o_data.csv /user/hadoop/datafolder/lots_o_data.csv
```

The following example would copy the same file from its distributed form in HDFS to the local (non-HDFS single server) file directory:

```
hdfs dgs -get /user/hadoop/datafolder/lots_o_data.csv lots_o_data.csv
```

The NameNode abstracts the true distributed location of the pieces of the file. This allows clients to address it with the familiar file folder nomenclature as if it was stored in one file on a single drive. The namespace automatically grows to incorporate new nodes, which add more storage space.

Files can be of any type and do not have to be a standard format. You can have unstructured files, such as word documents, and also very structured files, such as relational database table extracts. HDFS is indifferent to the level of structure in the data that resides in the files.

Many of the tools that interact with HDFS for analysis purposes do rely on structure. The structure can be applied as needed just before a file is read, known as schema-on-read. It could also be defined within the file itself as it is written. The second is what is called schema-on-write.