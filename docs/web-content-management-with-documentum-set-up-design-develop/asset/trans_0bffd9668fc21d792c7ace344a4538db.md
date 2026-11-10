
## B
## 新功能和增强功能（Release 5.3）

Most of the features added or enhanced by Documentum in its 5.3 release have been clearly mentioned under the appropriate sections of this book.
Documentum 在其 5.3 版本中添加或增强的大多数功能已在本書的相应章节中明确提及。

Additionally, this chapter categorizes and consolidates these features under one umbrella for a quick reference. The various changes made by Documentum in its WCM architecture, participating entities, and product suite as part of release 5.3 are mentioned in this chapter. However, please note that a detailed explanation of each of these enhancements is not within the scope of this book.
此外，本章将这些功能归类并整合在同一框架下，以便快速参考。Documentum 在 5.3 版本中对其 WCM 架构、参与实体和产品套件所做的各种更改在本章中有所提及。但是请注意，对这些增强功能中每一个的详细解释并不在本書的范围之内。

### B.1 Content Server Changes
### B.1 内容服务器更改

A few of the changes/enhancements made to Content Server in release 5.3 are:
在 5.3 版本中对内容服务器所做的一些更改/增强如下：

- Documentum has replaced Verity with Fast Search & Transfer™ (FAST™) as its new search infrastructure.
Documentum 已用 Fast Search & Transfer™ (FAST™) 取代 Verity 作为其新的搜索基础设施。
- A separate index agent and an index server are required in 5.3 for handling full-text indexing. While an index agent exports and prepares the documents in a Docbase repository for full-text indexing, the index server creates and maintains the full-text indexes and provides responses to Content Server full-text queries.
在 5.3 中处理全文索引需要单独的索引代理和索引服务器。虽然索引代理导出并准备 Docbase 仓库中的文档以进行全文索引，但索引服务器创建并维护全文索引，并对内容服务器的全文查询提供响应。
- Each repository requires its own index agent and index server.
每个仓库都需要自己的索引代理和索引服务器。
- In the new full-text indexing environment, all attributes of SysObjects and their custom subtypes are indexed. The content files associated with all the SysObjects are indexed as well and case-sensitive searching is not supported by Documentum in 5.3.
在新的全文索引环境中，SysObjects 及其自定义子类型的所有属性都被索引。与所有 SysObjects 关联的内容文件也被索引，并且 Documentum 在 5.3 中不支持区分大小写的搜索。
- In order to improve query performance, Documentum has introduced a new querying ability: FTDQL SELECT with syntax that is a subset of a SELECT statement. Using an FTDQL SELECT statement ensures that the query is executed against the full-text index rather than Docbase repository to achieve performance gains.
为了提高查询性能，Documentum 引入了一种新的查询功能：FTDQL SELECT，其语法是 SELECT 语句的子集。使用 FTDQL SELECT 语句可确保查询针对全文索引执行而不是 Docbase 仓库执行，从而获得性能提升。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_465_0.png)

Figure B.1: Execution of FTDQL and ordinary SELECT statements
图 B.1：FTDQL 和普通 SELECT 语句的执行

- A Trusted Content Services license is not required in order to control SSL communication between Content Server and client libraries. This feature is available in the standard Content Server package.
为了控制内容服务器与客户端库之间的 SSL 通信，不需要 Trusted Content Services 许可证。此功能在标准内容服务器包中可用。
- Enhancements have been made to ACLs (Access Control Lists) to provide better object-level permissions.
已对 ACL（访问控制列表）进行了增强，以提供更好的对象级权限。
- Apart from the five extended permissions shown in the table in figure 3.4 of Chapter 3, Documentum has provided an additional extended permission for deleting an object.
除了第 3 章图 3.4 表格中显示的五个扩展权限外，Documentum 还提供了用于删除对象的额外扩展权限。
- This new extended permission is different from the basic Delete permission in that it does not provide hierarchical Browse, Read, Relate, Version, or Write access.
该新的扩展权限与基本的 Delete 权限不同之处在于它不提供分层 Browse、Read、Relate、Version 或 Write 访问。

Apart from specifying the access permissions for basic and extended permissions, Documentum provides an ability to restrict these permissions as well in 5.3. There are two Access Restriction entries: AccessRestriction for restricting certain basic permissions and ExtendedRestriction for restricting certain extended permissions.
除了指定基本权限和扩展权限的访问权限外，Documentum 在 5.3 中还提供了限制这些权限的能力。有两个访问限制条目：AccessRestriction 用于限制某些基本权限，ExtendedRestriction 用于限制某些扩展权限。

The following example of an ACL on a document belonging to the HR department in an organization explains the utility of this feature:
以下示例说明了此功能的用途，该示例为组织中人力资源部门的文档上的 ACL：

### HR_ACL
### HR_ACL

```
Group name: HR_Department
    Permit Type: AccessPermit
    Permission Level: Delete

Group name: HR_Department_subgroup
    Permit Type: AccessRestriction
    Permission Level: write
```

This ACL generically provides delete access to the HR department group, but does not allow a particular subgroup within this HR department to update or delete the document. By using AccessRestriction, the system allows this sub-group to only browse, read, relate (annotate), and version the document.
此 ACL 通常为人力资源部门组提供删除访问权限，但不允许该人力资源部门内的特定子组更新或删除文档。通过使用 AccessRestriction，系统仅允许此子组浏览、读取、关联（注释）和版本化文档。

- **Flexibility to create groups as Dynamic Groups**: A Dynamic Group allows your application to find out whether the users specified in the group's membership list are considered members of this group or not by default when they establish a connection with the Docbase repository.
**将组创建为动态组的灵活性**：动态组允许您的应用程序在用户建立与 Docbase 仓库的连接时，默认情况下确定组成员列表中指定的用户是否被视为该组的成员。
  Say for example you assign a group abc as a Dynamic Group, setting its default membership behavior to treat its users as not members of this group. In the event of a user accessing the Docbase repository from a secure application, your application could add this user to the group but if the access is from a non-secure application, this user is not added to the group. Thus, using Dynamic Groups can assist you in setting up a group (or a role-based) security.
例如，假设您将组 abc 指定为动态组，将其默认成员行为设置为将其用户视为非该组成员。如果用户通过安全应用程序访问 Docbase 仓库，您的应用程序可以将其添加到该组，但如果访问来自非安全应用程序，则不会将该用户添加到该组。因此，使用动态组可以帮助您设置基于组（或基于角色）的安全性。
- Java programs have been supported in lifecycle Entry Criteria, Actions, Post Change, and for validation purposes.
Java 程序已在生命周期 Entry Criteria、Actions、Post Change 中得到支持，并用于验证目的。
- Improvements in workflows, such as support for work queues and a workflow timer for automatically resuming the suspended activities.
工作流的改进，例如支持工作队列和用于自动恢复挂起活动的工作流计时器。
- Support for Global Login Tickets.
支持全局登录票据。
- Login Tickets are ASCII-encoded strings used by applications in place of a user's password while establishing a connection with the Docbase repository. A Global Login Ticket is an extension to this; it is a Login Ticket that can be accepted by any server of a trusted Docbase repository.
登录票据是应用程序在建立与 Docbase 仓库的连接时用作用户密码的 ASCII 编码字符串。全局登录票据是此的扩展；它是可被受信任 Docbase 仓库的任何服务器接受的登录票据。
- The verity_locale key in server.ini file has become obsolete and the verity_location attribute in dm_server_config object type has been deprecated in the 5.3 release.
server.ini 文件中的 verity_locale 键已过时，并且 dm_server_config 对象类型中的 verity_location 属性在 5.3 版本中已被弃用。
- Introduction of Privileged Groups in the system: Members of these system groups have special privileges for performing specific operations that they do not individually have.
系统中引入了特权组：这些系统组的成员具有执行特定操作的特殊权限，而他们个人并不拥有这些权限。

Example: Members of the dm_browse_all group have the privilege to browse all the objects in the Docbase repository.
示例：dm_browse_all 组的成员有权浏览 Docbase 仓库中的所有对象。

### B.2 Object Types Changes
### B.2 对象类型更改

A few of the newly introduced object types in release 5.3 are:
在 5.3 版本中引入的一些新对象类型如下：

- dmc_jar: This is a subtype of dm_sysobject object type and represents a JAR file that is stored in the Docbase repository.
dmc_jar：这是 dm_sysobject 对象类型的子类型，表示存储在 Docbase 仓库中的 JAR 文件。
- dm_job_sequence: This is a subtype of dm_sysobject object type and stores details about a job that belongs to a set of jobs that have been scheduled to run in a specified sequence.
dm_job_sequence：这是 dm_sysobject 对象类型的子类型，存储属于已安排按指定顺序运行的一组作业的作业的详细信息。
- dmc_module: This object type represents a business module. Modules are covered later in the appendix.
dmc_module：此对象类型表示业务模块。模块将在附录后面介绍。
- The permanent_link attribute in the dm_relation object type has been deprecated and instead the new permanent_link and copy_child attributes of the dm_relation_type object type should be used.
dm_relation 对象类型中的 permanent_link 属性已被弃用，而应使用 dm_relation_type 对象类型的新 permanent_link 和 copy_child 属性。

### B.3 API and DQL Changes
### B.3 API 和 DQL 更改

A few of the enhanced Server API methods in release 5.3 are:
在 5.3 版本中增强的服务器 API 方法如下：

- connect and getLogin: Support Global login Tickets.
connect 和 getLogin：支持全局登录票据。
- grant and revoke: Support the enhancements made to Access Control Lists (ACLs).
grant 和 revoke：支持对访问控制列表（ACL）所做的增强。
- queue: Allows queuing of an event to a work item.
queue：允许将事件排队到工作项。

A few of the changes made to DQL in release 5.3 are:
在 5.3 版本中对 DQL 所做的一些更改如下：

- The SEARCH TOPIC clause has been deprecated and the FT_OPTIMIZER clause has been removed from the SELECT statement syntax.
SEARCH TOPIC 子句已被弃用，并且 FT_OPTIMIZER 子句已从 SELECT 语句语法中删除。
- A new DQL hint: UNCOMMITTED_READ has been added for MS SQL Server, DB2, and Sybase databases, for read-only queries for retrieving query results fast even if the tables in question are locked by another database session.
新增了一个 DQL 提示：UNCOMMITTED_READ，适用于 MS SQL Server、DB2 和 Sybase 数据库，用于只读查询，即使相关表被另一个数据库会话锁定也能快速检索查询结果。
- Introduction of the FTDQL SELECT statement described earlier in the appendix.
引入了附录前面描述的 FTDQL SELECT 语句。

### B.4 DFC Changes
### B.4 DFC 更改

A few of the changes made to DFC in release 5.3 are:
在 5.3 版本中对 DFC 所做的一些更改如下：

- Deeper integration with .NET.
与 .NET 的更深集成。
- Support for accessing Documentum Business Objects using Web Services.
支持使用 Web 服务访问 Documentum 业务对象。
- Support for lightweight Unified Client Facilities (UCF), providing content transfer functionality to clients based on the WDK framework, thus removing any impositions of having DFC available on the clients. It provides support for some of the basic content library services such as check-in/check-out, view/edit and import/export.
支持轻量级统一客户端设施（UCF），为基于 WDK 框架的客户端提供内容传输功能，从而消除了客户端必须提供 DFC 的限制。它支持一些基本的内容库服务，例如签入/签出、查看/编辑和导入/导出。
- The existing BOF deployment model using DBOR (Documentum business object registry) on each client machine has been deprecated.
使用每个客户端机器上的 DBOR（Documentum 业务对象注册表）的现有 BOF 部署模型已被弃用。
- Introduction of a ClassCastException that is thrown if one tries to cast a persistent object to a concrete class rather than its corresponding interface.
引入了 ClassCastException，如果尝试将持久对象强制转换为具体类而不是其对应的接口，则会抛出该异常。
- For example, casting to DfDocument (instead of IDfDocument) will throw a ClassCastException.
例如，将对象强制转换为 DfDocument（而不是 IDfDocument）将抛出 ClassCastException。

### B.5 BOF Changes
### B.5 BOF 更改

Following are a few of the changes made to BOF in release 5.3:
以下是 5.3 版本中对 BOF 所做的一些更改：

- TBOs (Type-based business objects) are associated with not just a type but also a Docbase repository. Thus, this does not necessitate having the same implementation of a TBO for all Docbase repositories.
TBO（基于类型的业务对象）不仅与类型相关联，还与 Docbase 仓库相关联。因此，这不需要为所有 Docbase 仓库使用相同的 TBO 实现。
- Support for dynamic deployment of an SBO (Service-based business object) from a central repository to multiple clients.
支持从中央仓库向多个客户端动态部署 SBO（基于服务的业务对象）。
- In order to implement TBOs in pre-5.3 releases, one needed to override multiple methods to perform the same task, such as: overriding save(), saveLock(), checkinEx(), etc. to customize check-in behavior.
为了在 5.3 之前的版本中实现 TBO，需要重写多个方法来执行相同的任务，例如：重写 save()、saveLock()、checkinEx() 等以自定义签入行为。
- In release 5.3, Documentum instead provides a set of supported do methods of DfSysObject such as doSave(), doCheckin(), etc. to achieve overriding. Additionally, Documentum provides a set of supported do methods in the DfPersistentObject and DfTypedObject classes for overriding purposes as well.
在 5.3 版本中，Documentum 改为提供 DfSysObject 的一组受支持的 do 方法，如 doSave()、doCheckin() 等来实现重写。此外，Documentum 还在 DfPersistentObject 和 DfTypedObject 类中提供了一组受支持的 do 方法用于重写目的。

### B.6 Application Builder and Application Installer Changes
### B.6 Application Builder 和 Application Installer 更改

A few of the enhancements made to Application Builder in release 5.3 are:
在 5.3 版本中对 Application Builder 所做的一些增强如下：

- Support for creating and managing Modules. Modules are units of executable code in a Docbase repository. An example could be Documentum business objects.
支持创建和管理模块。模块是 Docbase 仓库中的可执行代码单元。例如可以是 Documentum 业务对象。
- Support for procedures written as Java classes in the various lifecycle states.
支持在各种生命周期状态下以 Java 类编写的流程。
- Support for inserting user profiles and doc profiles, as well as work queues.
支持插入用户配置文件和文档配置文件，以及工作队列。

A few of the enhancements made to Application Installer in release 5.3 are:
在 5.3 版本中对 Application Installer 所做的一些增强如下：

- Support for installation of Modules.
支持模块的安装。
- Support for installation of lifecycles having procedures written as Java classes in the various lifecycle states.
支持安装具有在各种生命周期状态下以 Java 类编写的流程的生命周期。
- Support for installation of user profiles and doc profiles, as well as work queues.
支持用户配置文件和文档配置文件的安装，以及工作队列。
