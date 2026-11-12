To recap on the previous sections, Storage Spaces, in contrast, was a much simpler solution for aggregating local disks on a single computer and using these local resources to create virtual disks on which you could create volumes. Storage Spaces is not redundant and is still a direct component of Storage Spaces Direct, which we will look at in the next section.

Storage Spaces Direct provides several capabilities and benefits, some of which are as follows:

+   - Scales to 4 PB per cluster.
+   - USB flash drive witness.
+   - ReFS volumes compression and deduplication. The first 4 TB of a file is deduplicated, with 64 TB supported per volume.
+   - Mirror accelerated parity.

In this section, we introduced Storage Spaces Direct. In the next section, we will look at how it works.

# How does Storage Spaces Direct work?

The Storage Spaces Direct software-defined solution works by creating a cluster by connecting several servers (nodes) with local storage drives over Ethernet; no storage fabric, special cables, or adapter is required.

Storage Spaces Direct uses the following components:

+   - Failover clustering role
+   - Cluster Shared Volume (CSV) filesystem
+   - Storage spaces
+   - Software Storage Bus
+   - SMB 3

A core component of Storage Spaces is CSVs, which allow all nodes’ NTFS and ReFS volumes to have reads and writes simultaneously. There is no locking, masking, change of drive ownership, or dismounting and mounting volumes; this allows for faster failovers and provides load balanced distribution of requests to the storage from the compute.

To implement Storage Spaces Direct, you will need to enable Storage Spaces Direct on a failover cluster that you have created as a prerequisite step. The next stage is to aggregate the storage drives from each server in the cluster and create a software-defined pool of virtual shared storage. Volumes will be presented as drives to the VMs where you store data, such as virtual hard disk (VHD) files, which will then be created in the storage pool. The volumes use the CSV filesystem so that each server will see the volumes as locally mounted and can be accessed and perform operations simultaneously.