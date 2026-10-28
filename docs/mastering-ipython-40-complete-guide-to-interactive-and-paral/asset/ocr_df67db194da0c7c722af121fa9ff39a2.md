```
```python
for i in range(numFiles):
    filename = "infile" + i + ".txt"
    dv.targets = i % numEngines]
    asyncResults.append(dv.apply(countWords, fileName))

for r in asyncResults:
    results.append(r.get())
```
```

Readers will recognize this as similar to the scatter-gather method described earlier, the primary difference being that the data used by the individual engines is not primarily what was sent in the original call, but instead data that was stored in separate files.

The primary drawback to this form of data parallelism has to do with the underlying architecture of the physical machine. As a rough guide, most HPC machines will have local storage and remote storage. Local storage consists of RAM and a small hard drive. Remote storage consists of banks of hard drives and lots of tape. When a process starts on a node, the remote storage from that node is effectively blank. This means that any files used by that node have to be read from remote storage into local storage. Local and remote storage are connected by a network that is shared by all nodes regardless of who owns the processes running on them.

If, as in our toy example, a large number of nodes simultaneously try to open files in remote storage for reading, the network connecting local and remote storage will be overwhelmed by the need to transfer these files from remote storage to the individual nodes that need access. This is not, in itself, a fatal error. It will, however, kill performance, and is the sort of thing that will merit an angry e-mail from the system administrator.

In order to avoid such problems, it is necessary to control both what task accesses which file, but also where the files are physically located. This problem is beyond the scope of this book – if the reader is interested, **Hadoop Distributed File System (HDFS)** provides an interesting approach.

The preceding approaches all share a common feature – the data flows exclusively between the controller and the engines. Flows that are more sophisticated will be addressed in a later chapter.

# # Application steering

Application steering is the interactive manipulation of a program with the purpose of affecting its behavior. It allows the user to monitor and control their application during execution. The tools that IPython provides to enable some basic forms of application steering have been laid out in this chapter. This section will limit itself to describing some simple applications.