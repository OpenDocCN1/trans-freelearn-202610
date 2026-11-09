
## WCF LOB SQL 适配器

为了表示完整的销售订单，我们有一个名为 SalesOrder.xsd 的架构。并行形状完成后，对 SQL Server 执行全部三个操作后，我们就可以将它们的响应和原始销售订单头数据映射到最终的 SalesOrder 消息：

![](img/23d1402649b6528b085cd6b900a2c9c3_104_0.png)

这就是 LookupResponses_to_SalesOrder 映射的外观：

![](img/23d1402649b6528b085cd6b900a2c9c3_105_0.png)

如你所见，前面的映射相当直接，除了 ExecuteReaderResponse 节点外。如我们之前讨论的，Execute Reader 操作存在权衡。权衡在于我们的结果集将是无类型的，这意味着我们需要执行一些自定义工作来将此消息转换为强类型消息。为此我们需要编写一些 XSLT 并使用内联 XSLT 调用模板脚本 functoid 来映射 diffgram 内容：

```
<xsl:template name="AddressTemplate">
    <xsl:param name="param1" />
    <xsl:if test="count(//NewTable) > 0">
        <xsl:element name="Addresses">
            <xsl:for-each select="//NewTable">
                <xsl:element name="Address">
                    <xsl:if test="Name">
                        <xsl:element name="Name">
                            <xsl:value-of select="Name" />
                        </xsl:element>
                    </xsl:if>
                    <xsl:if test="AddressLine1">
                        <xsl:element name="AddressLine1">
                            <xsl:value-of select="AddressLine1"/>
                        </xsl:element>
                    </xsl:if>
                    <xsl:if test="AddressLine2">
                        <xsl:element name="AddressLine2">
                            <xsl:value-of select="AddressLine2"/>
                        </xsl:element>
                    </xsl:if>
                    <xsl:if test="City">
                        <xsl:element name="City">
                            <xsl:value-of select="City"/>
                        </xsl:element>
                    </xsl:if>
                    <xsl:if test="StateProvinceCode">
                        <xsl:element name="StateProvinceCode">
                            <xsl:value-of select="StateProvinceCode"/>
                        </xsl:element>
                    </xsl:if>
                    <xsl:if test="Country">
                        <xsl:element name="Country">
                            <xsl:value-of select="Country"/>
                        </xsl:element>
                    </xsl:if>
                </xsl:element>
            </xsl:for-each>
        </xsl:element>
    </xsl:if>
</xsl:template>
```

这就是该示例开发阶段的内容。最终的编排包含两个映射和全部三个并行执行的出站操作，看起来如下：

![](img/23d1402649b6528b085cd6b900a2c9c3_107_0.png)

现在解决方案的开发已完成，我们可以部署它并导入绑定文件，该文件位于本章源代码的 BindingFiles 文件夹中。此绑定文件创建一个发送端口，由三个操作共享，其绑定属性与创建架构时设置的属性相同。需要重点查看的是 WCF-Custom 适配器“常规”选项卡上的 SOAP Action 头属性：

```
<BtsActionMapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Operation Name="Select" Action="ViewOp/Select/Sales/vSalesOrderDetailProduct" />
<Operation Name="ufnGetContactInformation" Action="TableFunction/dbo/ufnGetContactInformation" />
<Operation Name="ExecuteReader" Action="GenericOp/ExecuteReader" />
</BtsActionMapping>
```

这提供了操作与 SOAP 操作之间的映射。请参阅第 1 章，了解 SOAP 操作和操作如何协同工作的详细信息。

当我们启用解决方案后，几秒钟后我们将收到包含销售订单数据的新消息。以下是处理后的销售订单示例：

```
<ns0:SalesOrder xmlns:ns0="http://Schemas.SalesOrder" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<Header>
<RevisionNumber>28</RevisionNumber>
<OrderDate>2001-07-01T00:00:00Z</OrderDate>
<DueDate>2001-07-13T00:00:00Z</DueDate>
<ShipDate>2001-07-08T00:00:00Z</ShipDate>
<Status>5</Status>
<SalesOrderNumber>SO43660</SalesOrderNumber>
<PurchaseOrderNumber>PO18850127500</PurchaseOrderNumber>
<AccountNumber>10-4020-000117</AccountNumber>
<CreditCardApprovalCode>115213Vi29411</CreditCardApprovalCode>
<SubTotal>1553.1035</SubTotal>
<TaxAmt>124.2483</TaxAmt>
<Freight>38.8276</Freight>
<TotalDue>1716.1794</TotalDue>
<Contact>
<FirstName>Takiko</FirstName>
<LastName>Collins</LastName>
<JobTitle>Purchasing Manager</JobTitle>
<ContactType>Store Contact</ContactType>
<Addresses>
<Address>
<Name>Main Office</Name>
<AddressLine1>6055 Shawnee Industrial Way</AddressLine1>
<City>Suwanee</City>
<StateProvinceCode>GA </StateProvinceCode>
<Country>United States</Country>
</Address>
</Addresses>
</Contact>
</Header>
<Items>
<Item>
<CarrierTrackingNumber>6431-4D57-83</CarrierTrackingNumber>
<OrderQty>1</OrderQty>
<UnitPrice>419.4589</UnitPrice>
<UnitPriceDiscount>0.0000</UnitPriceDiscount>
<LineTotal>419.458900</LineTotal>
<ProductName>Road-650 Red, 44</ProductName>
<ProductNumber>BK-R50R-44</ProductNumber>
<ProductSize>44</ProductSize>
<ProductWeight>18.77</ProductWeight>
<SellEndDate>2003-06-30T00:00:00Z</SellEndDate>
<SellStartDate>2001-07-01T00:00:00Z</SellStartDate>
</Item>
<Item>
<CarrierTrackingNumber>6431-4D57-83</CarrierTrackingNumber>
<OrderQty>1</OrderQty>
<UnitPrice>874.7940</UnitPrice>
<UnitPriceDiscount>0.0000</UnitPriceDiscount>
<LineTotal>874.794000</LineTotal>
<ProductName>Road-450 Red, 52</ProductName>
<ProductNumber>BK-R68R-52</ProductNumber>
<ProductSize>52</ProductSize>
<ProductWeight>17.42</ProductWeight>
<SellEndDate>2002-06-30T00:00:00Z</SellEndDate>
<SellStartDate>2001-07-01T00:00:00Z</SellStartDate>
</Item>
</Items>
</ns0:SalesOrder>
```

## 示例 3—查询通知和多个结果集

在本示例中，我们将介绍 ASDK SQL 适配器的两个高级功能。我们将使用查询通知来取代之前示例中用于检索尚未处理的销售订单的类型化轮询。我们还将使用多个结果集来检索销售订单信息并替换示例 2 中创建的查找操作。

尽管轮询入站操作非常常见，但轮询原则本身需要一些考虑。在没有数据返回时频繁轮询会浪费系统资源。在有数据返回时轮询不频繁意味着解决方案的效率不如预期。仔细考虑轮询间隔和轮询语句可以缓解这些限制，但通知是一种替代且通常更好的选择。

> ASDK SQL 适配器帮助信息中有一节很好地介绍了通知与轮询之间的差异，可在以下文章中找到：
> http://msdn.microsoft.com/en-us/library/dd788096(BTS.10).aspx

ADO.NET 为 ASDK SQL 适配器提供了一种通知选项：查询通知。查询通知构建在 SQL Server Service Broker 队列之上，并在 SQL Server 查询结果发生变化时向 BizTalk Server 引发事件。适配器封装了此功能，使用 ADO.NET SqlDependency 通知对象在查询结果更改时接收通知信息。

我们可以在“配置适配器”窗口和“绑定属性”选项卡中配置通知属性。可用的两个属性是 NotificationStatement 和 NotifyOnListenerStart。这些设置对我们在“使用适配器服务向导”中生成的 SQL 架构没有影响，并在配置接收位置时进行更详细的讨论。

![](img/23d1402649b6528b085cd6b900a2c9c3_110_0.png)

输入 Connection URI 后，我们需要单击“连接”，并确保将“服务（入站操作）”选为合约类型，因为查询通知是入站操作。接下来，我们选择“通知”操作并单击“确定”按钮。

![](img/23d1402649b6528b085cd6b900a2c9c3_111_0.png)

为“通知”操作创建的架构是通用的，其作用只是告知 BizTalk Server 解决方案有数据可供处理：

![](img/23d1402649b6528b085cd6b900a2c9c3_111_1.png)

架构的元素与 ADO.NET 关于查询更改接收到的信息相关，包含：

- **Info** – 此单个元素保存 System.Data.SqlClient.SqlNotificationInfo 枚举的值，提供有关适配器接收到的通知的附加信息。
- **Source** – 这将包含 System.Data.SqlClient.SqlNotificationSource 枚举的值，表示适配器接收到的通知的来源。
