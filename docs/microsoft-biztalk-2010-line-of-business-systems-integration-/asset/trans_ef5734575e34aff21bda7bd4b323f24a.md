
![](img/23d1402649b6528b085cd6b900a2c9c3_91_0.png)

**InboundId** 的值在适配器构建 TypedPolling 入站操作架构的命名空间时使用，也用于其生成的绑定文件中，该文件包含稍后将使用的 BizTalk 端口配置。此值可确保从同一数据库轮询的多个接收位置之间的连接 URI 唯一。非常重要的一点是我们配置接收位置时使用的 **InboundId** 必须与在 Consume Adapter Service 向导中使用的相同。否则，我们可以预期会出现 BizTalk 运行时错误。

**Binding Properties** 选项卡是我们设置支持类型化轮询操作的适配器属性的地方。以下是我们需要设置的一些重要属性：

- 1. **inboundOperationType** 需要为此场景设置。 由于我们有兴趣从 SQL Server 接收强类型结果集，因此需要将此属性的值填充为 **TypedPolling**。
- 2. **PolledDataAvailableStatement** 在运行时用于检查是否有可轮询的数据。 如果它在第一列中返回正值，则会执行 **PollingStatement** 语句。 这种方法的优点是我们可以提供一个轻量级查询来确定是否存在数据，而不是在执行时可能消耗更多资源的更复杂的轮询查询。 在此示例中，我们使用查询配置 **PolledDataAvailableStatement**，该查询检查尚未处理的销售订单：
- 3. 你可能已经注意到我们在语句开头设置了隔离级别，并在 select 查询上提供了 SQL 提示。 这是为了避免多服务器环境中的死锁。

```
SET TRANSACTION ISOLATION LEVEL READ COMMITTED
SELECT COUNT(*) FROM Sales.SalesOrderStatus WITH(UPDLOCK ROWLOCK READPAST )
WHERE OrderStatus = 'Pending' AND ProcessingIndicator IS NULL
```

- 4. PollingStatement 设置是每次 PolledDataAvailableStatement 返回正值时将执行的语句。 开发期间使用的相同 PollingStatement 必须在配置接收位置时使用。 这是必需的，因为该设置在运行时由适配器使用，以创建与 Consume Adapter Service 向导生成的架构匹配的消息。 为本示例的目的，我们将把 PollingStatement 设置为存储过程，该过程返回尚未处理的最初 100 个销售订单并将它们标记为已处理：

```
exec dbo.usp_GetSalesOrdersToProcess
```

![](img/23d1402649b6528b085cd6b900a2c9c3_92_0.png)

> 你也可以用分号分隔多个 T-SQL 语句。 例如，如果你有一个用于获取销售订单的存储过程和另一个用于将它们标记为已处理的存储过程，则 PollingStatement 可以是：
> 
> exec dbo.usp_GetSalesOrdersToProcess; exec dbo.usp_UpdateSalesOrdersToProcess

我们的存储过程使用新的 GUID 值更新 ProcessingIndicator 为 NULL 的最初 100 行的 ProcessingIndicator 列。 然后它返回 SalesOrderHeader 表中与这 100 行相关的数据。 我们选择在选择它们之前更新我们想要处理的记录。 通过这种方法，我们可以确保控制每次轮询事件想要返回给 BizTalk 的记录数量。
以下是存储过程：

```
CREATE PROCEDURE [dbo].[usp_GetSalesOrdersToProcess]
AS
SET nocount on
set xact_abort on
SET TRANSACTION ISOLATION LEVEL READ COMMITTED

declare @procIndicator uniqueidentifier
set @procIndicator = NEWID();

UPDATE Sales.SalesOrderStatus WITH (ROWLOCK)
SET ProcessingIndicator = @procIndicator,
    OrderStatus ='Processing'
FROM (
SELECT TOP 100 SalesOrderID
FROM  Sales.SalesOrderStatus WITH(UPDLOCK ROWLOCK READPAST)
WHERE ProcessingIndicator IS NULL AND OrderStatus ='Pending' ORDER BY SalesOrderID)
AS t1
WHERE (Sales.SalesOrderStatus.SalesOrderID = t1.SalesOrderID)

SELECT TOP 100 soh.[SalesOrderID], [RevisionNumber]
, [OrderDate], [DueDate]
, [ShipDate], [Status]
, [SalesOrderNumber], [PurchaseOrderNumber]
, [AccountNumber], [CustomerID]
, [ContactID], [BillToAddressID]
, [ShipToAddressID], [ShipMethodID]
, [CreditCardApprovalCode], [SubTotal]
, [TaxAmt], [Freight]
, [TotalDue], [Comment]
FROM Sales.SalesOrderHeader soh WITH(UPDLOCK ROWLOCK READPAST)
INNER JOIN Sales.SalesOrderStatus sos ON soh.SalesOrderID = sos.SalesOrderID
WHERE sos.ProcessingIndicator = @procIndicator
```

- 5. **PollingIntervalInSeconds** 指定 BizTalk Server 将运行设置在 **PolledDataAvailableStatement** 属性上的 SQL 语句的频率。 轮询间隔必须仔细设置，以避免在数据不经常更改时进行不必要的轮询，同时也要以足够高的频率轮询，以便及时捕获更改。 我们将其设置为 60 秒。
- 6. 我们示例中要介绍的最后一个属性是 **PollWhileDataFound**。 我们的目标是从 SQL Server 获取 100 个销售订单的批次。 将轮询语句返回的行数限制与 **PollWhileDataFound** 属性（设置为 true 时）结合使用是确保适配器创建多个较小消息的好做法。 该技术也很重要，因为 BizTalk Server 可以并行处理多个消息，而不是创建一个大消息。 设置为 true 时，**PollWhileDataFound** 属性指示适配器忽略 **PollingInterval** 属性并连续调用轮询语句。 对于我们当前的示例，我们将此属性设置为 **False**，以便我们能清楚地了解 BizTalk 的轮询行为。 如果我们持续处理所有数据，它将降低我们对 BizTalk 轮询周期的可见性。

以下是为我们的示例配置的 **Binding Properties** 选项卡：

![](img/23d1402649b6528b085cd6b900a2c9c3_94_0.png)

![](img/23d1402649b6528b085cd6b900a2c9c3_94_1.png)

在轮询场景中值得特别关注的属性是 **ReceiveTimeout**。 此属性表示适配器等待入站消息的时间量。 Microsoft 文档建议将此属性设置为最大允许值 24.20:31:23.6470000（24 天）。 设置这样大的值不会影响适配器的性能。 我们确实需要确保此值超过我们的轮询间隔，以确保在达到此超时阈值之前，我们的 BizTalk Server(s) 与 SQL Server 之间具有连接性。

有关 ASDK SQL 适配器属性的更多信息，请访问以下 TechNet 链接：http://technet.microsoft.com/en-us/library/dd787981(BTS.10).aspx

在设置绑定属性后，我们需要连接到我们的 SQL Server，选择 **Service (Inbound operations)**，选择根/类别，并添加 **TypedPolling** 操作：

![](img/23d1402649b6528b085cd6b900a2c9c3_95_0.png)

一旦我们按照前一图像所示配置了 Consume Adapter Service 向导，我们现在可以单击 **OK** 按钮。 Consume Adapter Service 向导随后将架构添加到 BizTalk Server 项目（在本示例中，架构名为 `TypedPolling.SalesOrders.xsd`），并创建包含物理接收端口和位置配置详细信息的绑定文件。

我们在 Typed Polling 场景中使用的架构包括存储过程的结果集的数组。 如果我们希望 BizTalk Server 进一步拆分它，架构很容易配置。 我们的存储过程被创建为每次轮询最多返回 100 个销售订单。 我们可以修改适配器创建的架构，以便 XmlDisassembler 管道将这些重复节点中的每一个拆分为单独的消息。 第一步是将 <Schema> 节点的 Envelope 属性设置为 Yes：

![](img/23d1402649b6528b085cd6b900a2c9c3_96_0.png)

然后，将 TypedPolling 节点的 Body Xpath 值设置为第一个 TypedPollingResultSet0。 这将告诉 XmlDisassembler 此记录下方的任何内容都应拆分为单独的消息：

![](img/23d1402649b6528b085cd6b900a2c9c3_96_1.png)

前两个步骤是在开发期间为 TypedPolling 架构配置其消息以便拆分所需设置的所有内容。

