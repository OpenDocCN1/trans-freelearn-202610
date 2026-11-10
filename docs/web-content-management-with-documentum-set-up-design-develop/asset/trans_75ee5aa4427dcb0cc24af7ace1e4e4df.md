
DFC 可被视为在 DMCL 之上编写的包装器，具有诸如数据验证等附加功能。

## 在 Documentum 业务对象框架 (BOF) 中可用的两种业务对象类型是什么？
基于类型的业务对象（简称为 TBO）和基于服务的业务对象（简称为 SBO）。

TBO 与特定对象类型绑定并扩展/覆盖其功能，而 SBO 与对象类型无关，并在系统中提供通用服务。在 5.3 版本中，TBO 同时绑定到对象类型和 Docbase 仓库。

## 请列举 Documentum WDK 框架提供的一些服务。
WDK 框架提供多种基本服务，例如错误处理、消息传递、内容传输和历史管理。

## 参照 ACL，请列出 Documentum 中所有可用的扩展权限。
Documentum 中可用的扩展权限有：Change State、Change Permission、Change Ownership、Change Location 和 Execute Procedure。在 5.3 版本中，Documentum 提供了用于删除对象的额外扩展权限。

此新的扩展权限与基本的 Delete 权限不同，因为它不提供分层的 Browse、Read、Relate、Version 或 Write 访问权限。

## 在 Alias Set 中可以为哪些类型或实体定义别名？
可以在 Alias Set 中为以下实体定义别名：user、group、user or group、cabinet path、folder path、permission set 以及 unknown（适用于没有实体匹配的情况）。

## 在 Documentum 中创建用户（dm_user 对象）所需的三个强制属性是什么？
为了在 Documentum 中创建用户，需要提供其用户名（user_name）、用户 OS 名称（user_os_name）和电子邮件地址（user_address）。

## 公共组和私有组之间有什么区别？
公共组是由 Documentum 中的 sysadmin 或 superuser 创建的组，而私有组是由具有创建组权限的任何用户创建的组。

## Registered Tables 在 Documentum 中有什么作用？
Registered Tables（dm_registered 对象）是 Documentum 系统外部的 RDBMS 表，不是 Content Server 的一部分，但需要在 Documentum 系统内访问。

## 请列出可以执行 Documentum 方法的执行代理。
Content Server（默认执行代理）、Method Server 和 Java Method Server。

## 哪个 Documentum 进程执行作业？
Content Server 中的 Documentum agent exec 进程执行作业。

## Web Publisher 如何能够连接到托管在 Content Server 上的 Docbases？
Web Publisher 主机中的 dmcl.ini 文件包含 DocBroker 名称和端口号的条目。Content Server 将其连接信息广播到该 DocBroker，然后可以建立连接。

## 请列举使用 Documentum Administrator 可以执行的一些系统管理任务。
Documentum Administrator 可用于创建和维护用户和组、对象类型和属性、管理审计跟踪、设置作业和方法，以及执行 DQL 查询和 Server API 命令。

## 哪个 Documentum 工具可用于在 Docbase 中创建文档的衍生件（替代格式）？
Content Rendition Services（CRS）。请注意，在 Documentum 5.3 版本中，Content Rendition Services 已拆分为两个产品：Document Transformation Services（DTS）和 Advanced Document Transformation Services（ADTS）。

## 通过 Site Caching Services（SCS）从 Docbase 发布时，哪些对象类型的子类型可以单独发布？
只有 dm_sysobject 对象类型的子类型才能由 SCS 从 Docbase 发布。

## Site Caching Services 中的两个主要组件是什么？
Site Caching Services（SCS）有两个主要组件：SCS Source 和 SCS Target。

## Site Deployment Services（SDS）通常用于什么目的？
SDS 通常用于将网站内容从 SCS 目标服务器部署并同步到多个本地或远程服务器。

## 是否可以在单个 Windows 主机上安装两个 Content Server？
不可以。只有在 UNIX 和 Linux 环境中才可能实现。

## 安装所有者和 Docbase 所有者之间有什么区别？
安装所有者是指其登录账户用于安装 Content Server 的用户。另一方面，Docbase 所有者是具有访问 Docbase 中底层 RDBMS 表权限的数据库用户账户。

## 在为 Docbase 提供 Docbase ID 时需要注意哪些限制？
如果特定 Content Server 上存在多个 Docbase，Docbase ID 应唯一，不应以零（0）开头，且应为 1 到 16777215 范围内的数字。

## Web Publisher Server Files 是否安装在 Content Server 主机上而不是 Web Publisher 应用服务器主机上？
是的。Web Publisher Server Files 包含一些由 Documentum Java Method Server 在 Content Server 上运行的 Java 方法 JAR。因此，这些需要安装在 Content Server 主机上，而不是 Web Publisher 应用服务器主机上。

## dmcl.ini 文件中用于启用连接池的键是什么？
connect_pooling_enabled。默认情况下为 false。

## 哪个文件存储 Content Server 所需的服务器信息？
位于 (Windows) Content Server 主机上的 server.ini 文件：
$DOCUMENTUM\dba\config\<Docbase name>

## 用于部署 Documentum Application Builder 创建的 DocApps 的工具是什么？
Documentum Application Installer（DAI）。

## Web Publisher Template、Rules 和 Presentation 文件是什么？
Web Publisher Template 是一个扁平的 XML 文件，其中包含内容作者需要填写的字段的 XML 元素。

Rules 文件是另一个 XML 文件，用于控制每个模板字段的行为和验证。

Presentation 文件是一个 XSL 样式表，用于设置模板内容文件的样式并以指定格式生成输出。

## 什么是 DocApp？
DocApp 是 Docbase 的打包单元，用于容纳各种 Docbase 对象，如对象类型、生命周期、工作流模板、权限集、别名集等。DocApp 中的对象可以使用 Documentum Application Installer（DAI）迁移/部署到其他 Docbase。

## 请列举 sysobject（dm_sysobject 对象类型）的一些子类型。
sysobject 的一些子类型有：job（dm_job）、method（dm_method）、document（dm_document）、policy（dm_policy）、process（dm_process）等。

## 自定义对象类型可以命名为 SELECT 或 DELETED 吗？
不可以。这些是 DQL（Document Query Language）中的保留关键字，不应用作对象类型的名称。

## Documentum 中对象类型属性可用的数据类型有哪些？
属性可用的数据类型有：Boolean、double、id、integer、string 和 time。

## 属性的输入掩码字符 # 表示什么？
哈希掩码字符（#）仅允许相关属性为数字值（0 到 9）。

## Value Assistance 是什么意思？
Value Assistance 用于显示特定属性的预定义值列表。用户可以从 Value Assistance 中显示的属性值集合中进行选择。请注意，此功能不适用于 Boolean 数据类型的属性。

## 参照 Registered Tables，引用 RDBMS 表的 Docbase 所有者时使用的别名是什么？
dm_dbo 是引用 Docbase 所有者时使用的别名。

## Documentum 用于解析别名的各种作用域有哪些？
Documentum 用于解析别名的各种作用域有：lifecycle scope、workflow scope、session scope、user scope 和 system scope。

## 在权限集的上下文中，Documentum 用于引用对象的当前所有者以及 Docbase 中所有用户的保留别名是什么？
dm_owner 是对象的当前所有者的别名，dm_world 是所有 Docbase 用户的别名。

## 生命周期策略对象在使用前有哪些不同状态？
生命周期策略对象的各种状态有：Draft（生命周期已创建或修改但尚未验证时）、Validated（生命周期已通过所有系统验证测试时）、Uninstalled 和 Installed（已验证并准备好供应用程序使用时）。

## 在生命周期状态图中可以创建哪两种类型的状态？
Normal 和 Exception 状态。Normal 状态用于对象的正常线性流程，而 Exception 状态表示异常流程或线性流程中的偏离。

## 请列举系统在生命周期状态中可以执行的一些标准操作。
系统在生命周期状态中可以执行的一些标准操作有：添加或移除版本标签、设置所有者、设置权限集、请求衍生件等。

## 是否可以在生命周期 Entry Criteria、Actions、PostChange 以及验证中指定 Java 类作为过程？
是的，在 Documentum 5.3 版本中可以实现。

## 请列举将对象通过生命周期各状态提升的几种机制。
将对象通过生命周期状态提升的几种机制有：Web Publisher Promote 菜单选项、工作流、promote server API 命令以及 BATCH_PROMOTE 管理方法。
