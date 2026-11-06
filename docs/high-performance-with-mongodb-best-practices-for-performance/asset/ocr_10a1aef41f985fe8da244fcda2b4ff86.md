+   - **“Free” memory (page cache)**: This refers to the remaining memory, typically used by the kernel to cache filesystem data in RAM to avoid repeated disk I/O. It provides a second layer of caching beyond WiredTiger’s cache and is particularly beneficial in scenarios where the WiredTiger cache is heavily utilized.

Understanding this memory distribution helps you better diagnose performance issues and make informed decisions about where to optimize system resources.

# Storage

Different types of storage devices typically have their own set of performance limitations. Physical storage devices generally fall into two main categories: mechanical HDDs and SSDs. Networked disks and virtual drives in cloud environments have other limitations.

The performance of physical storage devices is constrained by the following factors:

+   - **Bus interface speed**: The bandwidth of the device’s connection, such as SATA, PCIe, or NVMe, can cap the maximum available throughput.
+   - **Memory and cache size**: Most modern storage devices contain internal memory and cache. Insufficient caching can lead to bottlenecks when handling large amounts of data.
+   - **Fragmentation**: Data scattered across sectors or blocks can increase seek times and reduce performance for both HDDs and SSDs.
+   - **Read vs. write operations**: Write operations often require more processing than read operations, especially for SSDs, due to erase-before-write requirements.

![](bbox=[0.22, 0.64, 0.28, 0.73])

> **Erase-before-write**
> 
> SSDs use NAND flash memory, which requires data to be erased before new data can be written to the same location. This can slow down successive write operations, especially if many blocks need erasing. To mitigate this, SSD firmware and the operating system’s TRIM functionality can preemptively clean up unused cells to ensure available space, avoiding erase delays for future writes.

+   - **Sequential vs. random I/O**: Sequential reads and writes typically have higher throughput compared to random I/O due to reduced mechanical movement (for HDDs) or reduced overhead (for SSDs).