

## 自我评估模拟考试

欢迎来到《Splunk Enterprise 认证管理指南》的模拟考试章节！恭喜你在这条通往 Splunk Enterprise 认证管理员的路途上走到了这一里程碑。在本章中，我们将深入探讨一系列自我评估问答，旨在测试你的知识，并巩固全书所涉及的概念。

在继续之前，我想赞扬你在备考过程中所付出的努力和心血。你已经对 Splunk 平台、其管理功能及其强大特性有了全面的理解。这最后一章将为你提供一个评估学习进度、发现可能需要进一步学习或澄清的领域的机会。请记住——自我评估问题旨在挑战你对 Splunk Enterprise 管理概念的理解，因此请以开放的心态对待它们，并运用你在整个学习过程中积累的知识。慢慢来，仔细阅读问题，选择最恰当的答案。

完成自我评估问题后，你可以参照本章提供的答案来评估你的表现。检查任何错误答案并理解其背后的概念，对于进一步提升知识至关重要。请记住，本次自我评估并非为了模拟真实的认证考试。相反，它作为一种工具，用于衡量你的准备程度，并指出任何可能需要改进的地方。把它当作一个调整理解、巩固知识的机会。

原考试时长为 60 分钟、共 56 道题，要求你有效管理时间。考虑到你需要阅读和理解每道题的时间，为每道题分配大约 1 分钟是很重要的。

以下是考试建议时间管理的分配：

- **阅读、理解并回答每道题：** 每题 1-1.5 分钟
- **检查答案：** 考试结束前留出 3 分钟

遵循这种时间管理策略，你可以确保有足够的时间阅读和理解每道题、提供准确答案，并在提交考试前检查你的回答。

对于本自我评估章节，你将一共获得 56 道题，与原始考试格式一致。这些题目经过精心设计，涵盖 Splunk Enterprise 管理的各个方面，确保对你的知识和技能进行全面评估。请记住——自信源于充分准备。因此，请把最后一章当作巩固专业知识、以确定性迎接认证考试的机会。祝你顺利成为一名合格的 Splunk Enterprise 管理员，让我们开始自我评估问题吧！

## 模拟考试问题

- 1. 在 Splunk 中，哪个配置文件负责指定要收集和索引的数据输入？
  A. inputs.conf
  B. props.conf
  C. transforms.conf
  D. indexes.conf

- 2. 你是一名系统管理员：如何控制对 Splunk 中特定索引和资源的访问？
  A. 在 Splunk 服务器上配置防火墙规则。
  B. 在将数据索引到 Splunk 之前对数据进行加密。
  C. 设置身份验证机制，例如轻量级目录访问协议（LDAP）或安全断言标记语言（SAML）。
  D. 遵循 Splunk 基于角色的访问控制（RBAC），并创建一个可以配置为允许访问特定索引和资源的角色。角色又可以分配给用户。

- 3. 哪个 Splunk 组件负责将应用部署到转发器？
  A. 部署服务器（DS）
  B. 重型转发器（HF）
  C. 许可证管理器（LM）
  D. 搜索头（SH）

- 4. Splunk 索引器的作用是什么？
  A. 它对传入数据进行索引和存储，以供搜索和分析
  B. 它提供基于 Web 的界面，用于搜索和可视化数据
  C. 它管理用户身份验证和访问控制
  D. 它根据定义的条件运行计划搜索并生成警报

23. 在 Splunk 中，如何配置 DC 以与 DS 通信？（选择所有适用项）

- A. 在 deploymentclient.conf 文件中指定 DS 的地址
- B. 通过 Splunk Web 界面，路径为“设置 | 部署客户端”
- C. 在 DC 上执行 CLI 命令
- D. 联系 Splunk 支持以获取配置方面的帮助

24. 你能指出定义索引创建设置的配置文件吗？

- A. server.conf
- B. inputs.conf
- C. indexes.conf
- D. outputs.conf

25. 在 Splunk 索引器中，热桶和温桶存储的位置是什么？（选择配置设置）

- A. hotPath
- B. homePath
- C. coldPath
- D. warmPath

26. 定义索引最大大小的设置是什么？

- A. maxTotalDataSizeMB
- B. maxDataSizeMB
- C. indexMaxSize
- D. indexTotalSize

27. 从索引中删除数据的命令有哪些？（选择所有适用项）

- A. 通过 Splunk 搜索请求执行 delete 命令
- B. 通过 Splunk CLI 执行 delete 命令
- C. 通过 Splunk 搜索请求执行 splunk clean 命令
- D. 通过 Splunk CLI 执行 splunk clean 命令

28. Splunk 能否搜索索引的冻结桶？

- A. 能
- B. 不能

34. 在 Splunk 中，索引时（全局）优先级的最顶层目录是什么？
A. $SPLUNK_HOME/etc/apps/<app_name>/default
B. $SPLUNK_HOME/etc/users/<app_name>/local
C. $SPLUNK_HOME/etc/system/default
D. $SPLUNK_HOME/etc/system/local

35. Splunk 系统级配置默认存储在哪个位置？
A. $SPLUNK_HOME/etc/system/default
B. $SPLUNK_HOME/etc/apps/
C. $SPLUNK_HOME/bin
D. $SPLUNK_HOME/etc/apps/

36. 在搜索头集群（SHC）中，至少需要多少个搜索头（SH）？
A. 两个
B. 三个
C. 四个
D. 一个

37. 搜索因子（SF）配置在哪个 Splunk 组件上？
A. 搜索头（SH）
B. 索引器
C. 搜索头船长（SH captain）
D. 许可证管理器（LM）

38. 如果在索引器集群上设置了复制因子（RF）为 4，那么集群中至少需要多少个索引器实例？
A. 三个
B. 两个
C. 四个
D. 八个

39. 你能说出在搜索头（SH）上包含分布式搜索设置的两个配置文件的名称吗？

- A. outputs.conf
- B. server.conf
- C. serverclass.conf
- D. distsearch.conf

40. 在搜索头（SH）上，知识包复制的默认 replicationPolicy 值是什么？

- A. replicationPolicy = cascading
- B. replicationPolicy = classic
- C. replicationPolicy = mounted
- D. replicationPolicy = rfs

41. 在搜索头（SH）中，知识包复制的默认 maxBundleSize 限制是多少？

- A. 2 GB
- B. 4 GB
- C. 1 GB
- D. 3 GB

42. 在 Splunk 的摄取过程中，数据要经历三个阶段。以下哪项是 Splunk 中数据处理的这三个阶段？

- A. 输入
- B. 解析
- C. 索引
- D. 复制

43. 在 Splunk 中，文件/目录监控的配置应放在哪里？

- A. transforms.conf
- B. inputs.conf
- C. props.conf
- D. indexes.conf

44. 在 Splunk 中用于添加输入（inputs）的 Splunk CLI 命令是什么？

- A. splunk add monitor <path_to_data_source> -index <index_name> -sourcetype <sourcetype_name>
- B. splunk add inputs <path_to_data_source> -index <index_name> -sourcetype <sourcetype_name>
- C. splunk add files <path_to_data_source> -index <index_name> -sourcetype <sourcetype_name>
- D. splunk add directory <path_to_data_source> -index <index_name> -sourcetype <sourcetype_name>

45. 允许在请求中摄取任何文本负载的 HTTP 事件收集器（HEC）端点是什么？

- A. services/collector/event
- B. services/collector/raw
- C. services/collector/text
- D. services/collector/ack

46. 在 Splunk 中，如何有选择地摄取 Windows 输入的特定事件代码？

- A. 在 inputs.conf 中使用 Event_codes = <逗号分隔的代码列表>
- B. 在 inputs.conf 中使用白名单/黑名单设置
- C. 没有可能的解决方案进行选择性摄取，因此请在搜索时过滤代码
- D. 以上都不是

47. 你能识别数据通过哪些数据输入方式被摄取到 Splunk 中吗？（选择所有适用项）

- A. HEC——无代理输入
- B. 文件和目录监控
- C. 网络输入
- D. 使用技术插件（TA）进行数据收集

48. 在文件与目录输入中，如何递归地监控子目录？

- A. *（通配符）
- B. ...（目录段中的三点省略号）
- C. 将各个子目录单独配置为监控输入
- D. % %（在目录段中使用此表示法）

