
## 出站操作

在出站场景中，ASDK SQL 适配器作为发送端口基础设施的一部分，对数据库实体执行操作并将响应传回调用方。

下表显示了适配器支持的出站操作：

| 操作 | 描述 |
|---|---|
| 强类型存储过程 | 使 BizTalk 应用程序能够调用强类型存储过程。适配器生成的响应消息为强类型。 |
| 存储过程 | 与调用强类型存储过程类似。不同之处在于适配器生成的結果集的架构为弱类型。 |
| 标量函数 | 使 BizTalk 解决方案能够调用标量函数并接收标量值结果。 |
| 表值函数 | 使 BizTalk 解决方案能够调用表值函数并接收强类型结果。 |
| 表 | 使 BizTalk 解决方案能够对 SQL Server 表执行 Select、Insert、Update 和 Delete 操作，并还能够通过 VarText 和 VarBinary 操作类型设置 varchar(max)、nvarchar(max) 或 varbinary(max) 数据类型的表列。 |
| 视图 | 使 BizTalk 解决方案能够对 SQL Server 视图执行 Select、Insert、Update 和 Delete 操作，并还能够通过 VarText 和 VarBinary 操作类型设置 varchar(max)、nvarchar(max) 或 varbinary(max) 数据类型的视图列。 |
| 执行非查询 | 启用执行任何非 SELECT 语句，包括 DDL 语句。对于 INSERT、UPDATE 和 DELETE 语句，响应消息包含受影响的行数。对于其他语句，它始终为 -1。 |
| 执行读取器 | 启用对表和视图执行任何 SELECT 语句。您可以执行用分号分隔的多个语句。响应消息包含 DataSets 数组。 |
| 执行标量 | 启用执行任何 SELECT 语句。仅返回结果集中第一行的第一列。 |
| 复合操作 | 复合操作是适配器支持的一种特殊操作类型。它使 BizTalk 解决方案能够以任意顺序将多个 Insert、Update、Delete、Procedure 和强类型存储过程操作组合成在单个事务中执行的单个操作。 |

我们将在本章的练习部分展示最常用的出站操作。

## 在 Visual Studio 中使用 ASDK SQL 适配器

正如您在第 1 章中记得的那样，为项目中打算使用的操作生成架构和绑定文件是使 BizTalk 应用程序能够使用基于 ASDK 的适配器的重要步骤之一。此任务的首选工具是 Consume Adapter Service 向导，这是 ASDK 提供的设计时工具。要调用该向导，我们必须右键单击 Visual Studio 项目，选择 Add...，然后选择 Add Generated Items...。在 Add Generated Items 窗口中，我们需要选择 Consume Adapter Service 并单击 Add。一旦 Consume Adapter Service 窗口显示出来，第一步就是指定适当的绑定。ASDK SQL 适配器由 sqlBinding 表示——在 Consume Adapter Service 向导中执行其他操作之前，您必须在 Select a binding 下拉列表中选择它。

![](img/23d1402649b6528b085cd6b900a2c9c3_76_0.png)

然后，在选择绑定后，我们单击 Configure 按钮打开 Configure Adapter 窗口以指定数据库连接性和绑定属性。

## 提供凭据

Configure Adapter 窗口的 Security 选项卡允许我们指定访问 SQL Server 的凭据。ASDK SQL 适配器仅支持两种模式 – Windows 身份验证和 SQL Server 身份验证，但 Client credential type 下拉列表提供四种：

![](img/23d1402649b6528b085cd6b900a2c9c3_77_0.png)

这些选项是通用的，旨在满足不同基于 ASDK 的适配器的要求，因此让我们针对 ASDK SQL 适配器回顾这些选项：

- **None** – 此选项与 Windows 相同。
- **Windows** – 适配器将使用 Windows 身份验证连接到 SQL Server。
- **Username** – 适配器将使用 User name 和 Password 字段中提供的凭据连接到 SQL Server。
- **Certificate** – 即使选择了 Certificate 选项，适配器也会忽略它并使用 Windows 身份验证。映射到证书的用户不受支持，因此此选项不适用于 ASDK SQL 适配器。

请记住，只有用户有权访问的 SQL Server 对象才会显示在 Consume Adapter Service 向导上。因此，请确保在选择 Windows 身份验证模式时，Visual Studio 运行的帐户对项目所需的数据库对象具有适当的访问权限。对于 SQL Server 身份验证指定的用户名也适用相同的情况。

## 连接到 SQL Server URI 属性

URI Properties 选项卡是我们指定构建 SQL Server 连接字符串所需属性值的位置。

例如，要连接到名为 BT2010DEV-PC 的 SQL Server 默认实例上的名为 AdventureWorksCh2 的数据库，运行在默认端口 1433 上且没有故障转移服务器，我们可以指定以下内容：

![](img/23d1402649b6528b085cd6b900a2c9c3_78_0.png)

这将映射到以下连接 URI：

```
mssql://BT2010DEV-PC//AdventureWorksCh2?
```

> 如果这样更方便，您也可以在主 Consume Adapter Service 向导窗口的 URI 文本框中输入连接字符串，而不是使用 URI Properties 选项卡。

一般形式下，SQL Server 的 URI 连接字符串如下所示：

```
mssql://[Server_Name[:Portno]]/[SQL_SERVER_Instance_Name]/[Database_Name]?FailoverPartner=[Partner_Server_Name]&InboundId=[Inbound_ID]
```

其中：

- 服务器名称 (Server_Name)、端口 ([:Portno]) 和实例名称 ([SQL_SERVER_Instance_Name]) 确定要连接的 SQL Server 实例，而数据库名称 ([Database_Name]) 指定数据库。如果您连接到 SQL Server 的默认实例，可以将 SQL_SERVER_Instance_Name 部分留空。
- 如果 SQL Server 实例还具有在主服务器无响应时由适配器使用的故障转移伙伴，则使用伙伴服务器名称 ([Partner_Server_Name])。
- 需要注意的连接 URI 的最后一部分是 InboundID ([Inbound_ID])。使用 TypedPolling 操作生成唯一架构并唯一标识相应的接收位置时必须设置它。如果我们使用 InboundID 生成架构，那么在相应的接收位置配置中提供相同的 InboundID 也是至关重要的。如果我们不这样做，将会收到运行时错误，指示 BizTalk 无法按消息类型找到文档规范。
- 在内部，LOB SQL 适配器使用 URI 和绑定属性的组合来构建最终的 ADO.NET 连接字符串，如以下博客文章中所述：

```
http://connectedthoughts.wordpress.com/2011/01/18/how-the-wcf-sql-adapter-builds-the-ado-net-connection-string/
```

## 绑定属性

Binding Properties 选项卡是我们配置适配器运行时行为的位置：

![](img/23d1402649b6528b085cd6b900a2c9c3_79_0.png)

此选项卡上显示的属性反映在 Consume Adapter Service 向导生成的绑定文件中。对于出站操作，我们通常可以接受默认值，因为默认值对于大多数情况来说是足够的。在默认值不令人满意的情况下，我们始终可以使用 BizTalk Admin Console 对物理端口进行调整。

对于入站操作，情况稍微复杂一些。由于入站操作的性质，Consume Adapter Service 向导不提供默认值，我们必须显式提供配置设置。我们可以在设计时完成，然后导入绑定文件以创建配置的接收位置，或者稍后使用 BizTalk Admin Console 提供设置。唯一的例外是用于配置 TypedPolling 入站操作的 PollingStatement 属性。此属性直接影响 Consume Adapter Service 向导为操作生成的架构，因此必须在开发时设置。当我们部署应用程序时，我们必须确保接收位置上设置的 PollingStatement 属性与我们在开发时使用的一致。如果这听起来令人困惑，请继续阅读。我们在示例部分提供了对 TypedPolling 操作和适用配置属性的详细说明。

## 生成架构

完成连接性和绑定设置后，我们单击 OK 关闭 Configure Adapter 窗口并返回到主 Consume Adapter Service 窗口。此时，Configure a URI 文本控件包含基于我们在 Configure Adapter 窗口的 URI 选项卡上指定的属性构建的连接 uri 字符串。

![](img/23d1402649b6528b085cd6b900a2c9c3_80_0.png)

现在我们单击 Connect 按钮；如果连接成功建立，Consume Adapter Service 向导将授予我们访问适配器公开的操作的权限：
