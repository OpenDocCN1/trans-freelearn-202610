
要在搜索头上添加单站点集群索引器，请在搜索头的 $SPLUNK_HOME/bin 目录下执行以下命令。幕后配置设置将被添加到 $SPLUNK_HOME/etc/system/local 目录下的 server.conf 文件中：

```
./splunk add cluster-manager <scheme>://<host>:<port> -secret <indexers-pass4symmkey> -multisite false
```

对于每个集群管理器，都必须重复执行此命令。

让我们看看此命令的各个要素：

- `<scheme>://<cluster-manager-servername>:<management-port>` 是集群管理器的详细信息
- `<indexer-pass4symmkey>` 是索引器集群的 pass4SymmKey/机密/安全密钥

举例来说，假设您之前已执行过添加集群管理器地址 SplunkManager01.example.com 的命令，则 server.conf 文件现在显示以下内容：

```
[clustering]
mode = searchhead
manager_uri = https://SplunkManager01.example.com:8089
pass4SymmKey= encrypted-secreteast
```

如需更多选项（例如列出、编辑或删除集群管理器），您可以使用帮助命令。例如，在 $SPLUNK_HOME 的 /bin 目录下执行 ./splunk help cluster-manager。

同样，如果您想了解如何编辑和/或删除搜索服务器，可以按如下方式使用帮助命令：./splunk help search-server。此操作应在 $SPLUNK_HOME 的 /bin 目录下完成。

我们现在已经了解了如何在搜索头中同时包含集群和非集群索引器，以配置分布式搜索。接下来，下一节将指导我们使用 Splunk Web 界面配置分布式搜索的过程。

## Splunk Web

使用 Splunk Web，我们可以创建与上一节中使用 CLI 所做的相同配置。让我们看看图 7.3 中的菜单选项。

以管理员用户身份登录 Splunk Web，导航到右上角的“设置”并单击它。然后，您将在“分布式环境”下找到以下菜单项：

图 7.4 包含以下文本字段：对等 URI（peer-URI:mgmt_port 格式）、远程用户名和远程密码，您可以在其中手动输入需要添加到搜索头的索引器的详细信息。填写这些字段并单击“保存”。对于每个搜索对等体，重复图 7.3 和图 7.4 中所示的相同步骤，将它们添加到搜索头。

在下一节中，我们将向搜索头添加集群索引器。

## 添加集群索引器

在 Splunk Web 中，单击“设置”菜单，然后单击“索引器集群”，如图 7.3 所示，这将打开一个新页面，其中包含“启用索引器集群”按钮。单击该按钮并选择“搜索头节点”单选按钮，如图 7.5 所示。

![](img/5319cbd6f078c6cda7fac6347f77a07d_133_0.png)

图 7.5：分布式搜索 – 在搜索头上启用集群

图 7.5 显示了在搜索头上启用索引器集群的选项。最后，单击“下一步”按钮。将打开一个表单，如图 7.6 所示。

- 要查看知识包中包含的允许配置的完整列表，请参阅搜索头的 distsearch.conf 文件中的 [replicationAllowlist] 和 [replicationSettings:refineConf]。要检查包复制状态，可以在搜索头上执行 splunk show bundle-replication-status 命令，或者使用监控控制台，其搜索菜单下有一个仪表板。

如果公共知识包版本无法在所有搜索对等体之间复制，可能会导致搜索暂时中断。当搜索进程尝试跨对等体同步知识包时，可能会暂停几秒钟。如果建立公共包失败，搜索进程最终将放弃并继续，而不会实现同步。在这种情况下，Splunk 管理员将采取必要措施，识别缓慢的搜索对等体并将其隔离在搜索头上，以防止该对等体参与未来的搜索。隔离后，搜索对等体确保已正在运行的搜索完成。隔离操作仅暂停向该对等体复制知识包。

可以通过 Splunk Web 单击“设置 | 分布式搜索 | 搜索对等体”来实现隔离。也可以通过 CLI 完成。成功恢复后，请解除对该对等体的隔离，以便其参与未来的搜索。

以下是 CLI 隔离和解除隔离命令：

```
./splunk edit search-server -auth <user>:<password> <host>:<port> -action [quarantine|unquarantine]
```

让我们更详细地了解一下：

- <user>:<password> 是搜索头的管理员级凭据
- <host>:<port> 是搜索对等体的主机和管理端口
- -action 的值为 quarantine 或 unquarantine

最后总结一下；知识包是一个存档文件，由搜索头通过后台复制过程与搜索对等体共享。知识包包含知识对象，搜索对等体在将搜索结果返回给搜索头之前会将这些知识对象应用于搜索结果。默认情况下，包大小限制适用，为了减小其大小，可以将某些知识对象添加到搜索头的 replicationDenyList 设置中。要暂停向特定对等体复制包，可以隔离该搜索对等体。

让我们在下一节中总结我们所学到的内容。

3. 在搜索头集群中，搜索头首领组件是否是必需的？
A. 是
B. 否

4. 您被分配到一个关键项目，其中配置了搜索头集群来搜索多个非集群索引器。这是否利用了分布式搜索概念？
A. 是
B. 否

5. 找出搜索头上包含索引器详细信息的配置文件。
A. serverclass.conf
B. server.conf
C. distsearch.conf
D. distributedpeers.conf

6. 知识包包含什么？（选择所有适用项。）
A. 用户、知识对象和应用
B. 知识对象，如事件类型、标签和已保存搜索
C. Splunk 二进制文件
D. 有关搜索过程的 Splunk 内部日志

7. 知识包的默认复制策略是什么？
A. 级联
B. RFS
C. 经典
D. 挂载

8. 知识包已经变得太大，超过了 maxBundleSize。您是一名系统管理员；您会怎么做？
A. 增加 maxBundleSize
B. 从搜索头删除较大的应用
C. 将不需要的文件/应用添加到 replicationDenyList
D. 什么都不做；搜索头仅共享 .delta 文件

## 第 2 部分：
Splunk 数据管理

本 Splunk 9.x Enterprise 认证管理员指南的第 2 部分深入探讨 Splunk 数据管理的世界。它将使您具备高效地将数据导入 Splunk、配置数据输入、执行解析和转换、提取有意义的字段以及使用查找来增强数据所需的技能。自评估模拟考试确保您可以有效地应用所学知识。

此部分包含以下章节：

- 第 8 章，数据导入
- 第 9 章，配置 Splunk 数据输入
- 第 10 章，数据解析与转换
- 第 11 章，字段提取与查找
- 第 12 章，自评估模拟考试

如果您想深入了解这些阶段的详细概述，以及队列和管道，请查看 https://tinyurl.com/bdzb cetp。请注意，这对于认证考试来说完全是可选的。

我们现在已经研究了主要的数据摄取阶段和元数据字段。接下来，我们将探索 Splunk Web 中可用的 Splunk Enterprise 的“添加数据”功能。

## Splunk Web – 添加数据功能

“添加数据”功能位于 Splunk Web 的“设置”菜单下。只需打开此菜单并单击“添加数据”，如图 8.3 所示。

![](img/5319cbd6f078c6cda7fac6347f77a07d_149_0.png)

图 8.3：Splunk Web – 添加数据

单击“添加数据”将打开一个新的网页，提供三种将数据导入 Splunk 的方法，如图 8.4 所示。

![](img/5319cbd6f078c6cda7fac6347f77a07d_149_1.png)

图 8.4：将数据导入 Splunk 的方法

