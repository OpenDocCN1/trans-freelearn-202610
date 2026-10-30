

根据 AWS 文档，每个数据库实例允许最多五个直接只读副本，这有助于扩展读取性能。只读副本由于异步复制具有非常低的 RPO 值。在主实例故障的情况下，它们可以提升为读写数据库实例。这可以快速完成，并且提供相当低的 RTO 值。

在下一节中，我们将了解亚马逊自己的数据库引擎 Amazon Aurora。

## 使用多主功能向 Amazon Aurora 写入

Amazon Aurora 是亚马逊开发的最可靠的关系型数据库引擎，以简单且经济高效的方式提供速度。Aurora 使用单主实例集群和零个或多个副本。Aurora 的副本可以让你同时获得 RDS 中只读副本和多可用区实例的优势。Aurora 使用共享集群卷进行存储，可供集群的所有计算实例使用（最多 64 TiB）。这使得 Aurora 集群能够更快地配置，并提高可用性和性能。Aurora 使用基于 SSD 的存储，提供高 IOPS 和低延迟。与其他 RDS 实例不同，Aurora 不要求你分配存储；它基于你使用的存储。

Aurora 集群具有多个端点，包括 **Cluster Endpoint** 和 **Reader Endpoint**。如果没有副本，则集群端点与读取器端点相同。如果有副本可用，则读取器端点会在读取器端点之间进行负载均衡。集群端点用于读写，而读取器端点用于从集群读取。如果添加更多副本，则 AWS 会在幕后为新副本管理负载均衡。

当故障转移发生时，副本会被提升为读写模式，这需要一些时间。这可以在 Aurora 集群的 **Multi-Master** 模式下避免。这允许多个实例同时执行读写操作。

## 在 Amazon Redshift 上存储列式数据

Amazon Redshift 不用于实时事务用途，而是用于数据仓库用途。它旨在支持 PB 级的海量数据。它是用于分析目的的基于列的数据库，用于长期处理、趋势分析和聚合。**Redshift Spectrum** 可用于在不将数据加载到 Redshift 集群的情况下查询 S3 上的数据（尽管需要 Redshift 集群）。它不是 OLTP，而是 OLAP。**AWS QuickSight** 可与 Redshift 集成用于可视化，提供类 SQL 接口，允许你使用 JDBC/ODBC 连接查询数据。

Redshift 在 VPC 中的一个可用区使用集群架构，节点之间具有更快的网络连接。它并非按设计具有高可用性，因为它与可用区紧密耦合。Redshift 集群有一个 **Leader Node**，该节点负责客户端与集群计算节点之间的一切通信、查询规划和聚合。**Compute Nodes** 负责运行由 leader 节点提交的查询并存储数据。默认情况下，Redshift 使用公共网络与外部服务或任何 AWS 服务通信。通过 **Enhanced VPC Routing**，可通过自定义网络设置进行控制。

## Amazon DynamoDB 作为 NoSQL 数据库即服务

Amazon DynamoDB 是 AWS 内的 NoSQL 数据库即服务产品。它是完全托管的键值和文档数据库。通过其端点访问 DynamoDB 很简单。输入和输出吞吐量可以手动或自动方式管理或扩展。它还支持数据备份、时间点恢复和数据加密。我们在本章中不会介绍 DynamoDB 表结构或键结构，因为这对认证考试不是必需的。然而，对它们有一个基本的了解是好的。如需更多详细信息，请参阅此处可用的 AWS 文档：https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SQLtoNoSQL.html。

## 总结

在本章中，我们了解了亚马逊的各种数据存储服务，以及如何通过各种策略保护数据并使用这些服务。如果你正在处理机器学习用例，那么你可能会遇到需要为你的需求选择有效数据存储服务的场景。

在下一章中，我们将学习存储数据的处理。

## Questions

+ 1. 要设置 S3 存储桶存储的区域，你必须先创建存储桶，然后再单独设置区域。
+ A. True
+ B. False

+ 2. 是否必须将源存储桶和目标存储桶都放在同一区域才能复制 S3 存储桶的内容？
+ A. True
+ B. False

+ 3. 默认情况下，存储桶中的对象是私有的。
+ A. True
+ B. False

+ 4. 根据 AWS，S3 对象是不可变的，你只能执行 put 和 delete。重命名是使用不同名称对同一对象进行 GET 和 PUT。
+ A. True
+ B. False

+ 5. 如果用户在同一个存储桶中存储了未版本化和已版本化的对象，那么用户只能删除未版本化的对象。已版本化的对象无法删除。
+ A. True
+ B. False

> **Answer**
> 版本控制适用于整个存储桶而不是对象。如果在存储桶上启用了版本控制，则只能暂停它；无法禁用它。

6. 对删除标记执行简单删除请求将：
A. 删除删除标记
B. 创建删除标记的副本
C. 不删除删除标记，但会创建另一个删除标记
D. 删除对象的原始版本

7. RDS 多可用区实例可通过提高扩展性和性能。
A. True
B. False

Answer
RDS 多可用区与扩展性和性能无关。它用于故障转移。

8. EFS 使用什么协议？
A. SMB
B. NFS
C. EBS
D. HTTP

9. EFS 支持哪些操作系统？
A. 仅 Linux
B. 仅 Windows
C. Windows 和 Linux 均支持
D. Windows 和 Linux 均不支持

10. EFS 是否为私有服务？
A. 是。
B. 否，它是公共服务。
C. 它既是私有服务也是公共服务。
D. 它既不是私有服务也不是公共服务。

11. 以下哪两个是正确的？

- A. Multi-AZ:Same Region::Read Replica:Multiple Region
- B. Multi-AZ:Multiple Region::Read Replica:Same Region
- C. Multi-AZ:Synchronous Replication::Read Replica:Asynchronous Replication
- D. Multi-AZ:ASynchronous Replication::Read Replica:Synchronous Replication

12. 以下哪个是正确的？

- A. 只读副本是只读实例。
- B. 只读副本是读写实例。
- C. 只读副本是仅写实例。
- D. 只读副本是只读实例，直到提升为读写。

13. EFS 可从何处访问？

- A. 仅在 VPC 内部
- B. 通过 AWS 端点
- C. 任何具有公共互联网连接的位置
- D. 在 VPC 内部或通过混合网络连接到该 VPC 的任何本地位置

14. 以下哪三个是正确的？

- A. 实例存储卷是持久存储。
- B. 实例存储卷是临时存储。
- C. 如果发生硬件故障，存储在实例存储卷上的数据可能会丢失。
- D. 当 EC2 实例重启时，存储在实例存储卷上的数据可能会丢失。
- E. 当 EC2 实例停止并启动时，存储在实例存储卷上的数据可能会丢失。

> **Answer**
> 硬件故障可能会更改底层主机。因此，实例存储卷没有保证。当你停止实例并再次启动时，由于主机更改，实例存储卷会丢失。实例重启与停止和启动不同；它意味着操作系统重启。

15. 为了使用 EC2 和 EBS 启用静态加密，你需要…

- A. 在创建 EBS 卷时配置加密。
- B. 使用适当操作系统的文件系统配置加密。
- C. 使用 X.509 证书配置加密。
- D. 将 EBS 卷挂载到 S3，然后使用存储桶策略加密存储桶。

16. 以下哪一项是你在 EBS 快照上无法执行的操作？

- A. 从快照创建镜像。
- B. 从快照创建 EBS 卷。
- C. 与另一个 AWS 账户共享快照。
- D. 对加密快照制作未加密的副本。

17. 使用 EBS，你需要执行以下操作（选择两个）。

- A. 从另一个加密卷的快照创建加密卷。
- B. 从加密快照创建加密卷。
- C. 通过创建未加密快照的加密副本，从未加密快照创建加密快照。
- D. 加密现有卷。

> **Answer**
> 
> 有关 EBS 的更多信息，请访问以下链接：https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#EBSEncryption_c%20onsiderations。

