# Extent file system (XFS)

The MongoDB production notes recommend XFS for production deployments. XFS generally delivers better performance for workloads with high throughput and concurrent read/write operations. It handles large files and fragmented data more efficiently than ext4 and tends to better manage large numbers of flush operations, exhibiting fewer I/O stalls during checkpoints. XFS also supports larger file and filesystem sizes.

Originally developed by Silicon Graphics, Inc. (SGI) in 1994, XFS was ported to Linux in 2001 and integrated into the mainline kernel in 2004 (version 2.6). Red Hat adopted XFS as the default filesystem for RHEL 7 in 2014, citing its scalability and performance for enterprise workloads.

# Fourth extended filesystem (ext4)

If the Linux distribution you are using supports XFS, use that. If not, ext4 remains a solid choice. ext4 is a mature, widely used filesystem and has long been the default on many Linux distributions. While ext4 used to be more widely available than XFS, the gap has closed in recent years. The performance gap between ext4 and XFS has also narrowed over time as operating systems, kernels, and MongoDB itself have evolved.

# Other file systems

While MongoDB can run on other filesystems, their features sometimes overlap with those already provided by MongoDB and WiredTiger, leading to inefficiencies:

+   - **Network File System (NFS)**: Not recommended for MongoDB. See the MongoDB production notes for more information.
+   - **B-tree File System (Btrfs)**: Can struggle with write-heavy workloads. Its **copy-on-write** (CoW) design can cause write amplification, where a 100B document at the database level could result in many times more data being written to the storage device. Btrfs also implements mechanisms that already exist in WiredTiger.
+   - **Zettabyte File System (ZFS)**: Better suited for large, sequential data such as backups or media. For MongoDB’s small, random reads/writes, ZFS can be slower than XFS or ext4.
+   - **Ceph File System (CephFS)**: Designed for distributed storage clusters and cloud-native apps. MongoDB already supports these use cases via sharding and replication.

In general, XFS or ext4 are the most reliable choices for MongoDB deployments, offering the best balance of performance, stability, and compatibility.