
图15.17：展开FactInternetSalesLive列

16. 从画布中选择新的Merge表，然后点击加号按钮（+）。
17. 从上下文菜单中选择“选择列”。
18. 选择ProductKey、EnglishProductName和SalesAmount列。
19. 点击“确定”。

下图显示了上述步骤：

![](img/5d2bce74fd6fceebed14fd1488bf7088_772_0.png)

图15.18：在Datamarts的图表视图中选择列

20. 再次点击加号按钮（+），然后点击**分组依据**转换。
21. 点击**高级**选项。
22. 点击**添加分组**按钮。
23. 确保在**分组依据**部分中选择了**ProductKey**和**EnglishProductName**。
24. 在**已售产品计数**中输入第一个聚合的名称，并选择`Count rows`操作。
25. 点击**添加聚合**按钮。
26. 在“新列名称”中输入**Internet Sales**，选择**求和**作为操作，并从**列**下拉菜单中选择**SalesAmount**。
27. 点击**确定**。

下图显示了上述步骤：

图15.19：在Datamarts的图表视图中使用分组依据操作

如你所见，我们无需编写任何代码就创建了一个汇总表。现在我们可以双击“Visual query 1”选项卡，将其重命名为“Products Sold Summary”。下图显示了结果：

图15.20：在Datamarts中重命名可视化查询

点击“在Excel中打开”按钮，可将查询作为Excel文件下载。该Excel文件包含一个Power Query查询，该查询使用上述可视化查询生成的原生查询连接到底层Azure SQL数据库。下图显示了在通过凭据后结果被加载到Excel中的情况：

![](img/5d2bce74fd6fceebed14fd1488bf7088_774_0.png)

图15.21：传递凭据后结果被加载到Excel中

我们可以从可视化关系图中获取原生查询，这将引出下一小节。

### 获取底层的原生查询

在Datamarts中，我们可以通过右键单击某个步骤，然后从上下文菜单中选择“查看数据源查询”来轻松获取每个转换步骤生成的原生查询。下图显示了上述步骤和生成的原生查询：

![](img/5d2bce74fd6fceebed14fd1488bf7088_775_0.png)

图15.22：从Datamarts的图表视图中获取原生查询

生成的原生查询的一个用途是后续将其复用来创建新表或SQL查询。

如上图所示，我们可以查看查询计划以更详细地调查查询。下一节将解释具体方法。

### 查看查询计划

Power Query Online在2023年公开预览的最酷新功能之一是**查看查询计划**。此功能提供有关查询评估的更多详细信息，可用于查询性能调优，以及从查询折叠的角度查看问题出在哪里。我们可以通过右键单击转换步骤并从上下文菜单中选择**查看查询计划**选项来访问查询计划，如下图所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_776_0.png)

图15.23：从Datamarts的图表视图中查看转换步骤的查询计划

这会打开“查询计划”窗口，我们可以在其中点击“查看详细信息”以了解更多关于所选节点的信息：

![](img/5d2bce74fd6fceebed14fd1488bf7088_776_1.png)

图15.24：查看Datamarts中查询计划节点的详细信息

我们可以点击“导出”按钮以JSON格式导出查询计划。这部分内容留给你自己测试。

## SQL查询体验

如果你是一名经验丰富的SQL开发人员，并且想要分析运行T-SQL语句的Datamart，那么本小节正适合你。请按照以下步骤对承载Datamart数据的底层SQL数据库运行查询：

1. 在Power BI Datamart编辑器中切换到“查询”视图。
2. 点击“新建查询”下拉按钮。
3. 从菜单中选择“新建SQL查询”。这将在“资源管理器”窗格的“查询”部分下创建“SQL查询1”。
4. 输入SQL语句。
5. 点击“运行”按钮。

下图显示了上述步骤：

![](img/5d2bce74fd6fceebed14fd1488bf7088_777_0.png)

图15.25：用于分析Datamarts的SQL查询体验

## 在SQL客户端工具中分析Datamarts

如前所述，Power BI Datamarts会将准备好的数据加载到Azure SQL数据库中。我们可以使用SQL Server Management Studio（SSMS）或Azure Data Studio等SQL客户端工具连接到底层数据库。以下步骤说明如何使用SSMS连接我们之前创建的Datamart的SQL端点：

1. 在Power BI服务中，将鼠标悬停在所需Datamart上，点击“更多选项”省略号按钮。
2. 点击“设置”。
3. 展开“服务器设置”。
4. 点击“复制”按钮以复制连接字符串。

下图显示了上述步骤：

![](img/5d2bce74fd6fceebed14fd1488bf7088_778_0.png)

图15.26：复制Datamart的连接字符串

5. 在SSMS中，点击“连接”下拉按钮。
6. 点击“数据库引擎”选项。
7. 将复制的连接字符串粘贴到“服务器名称”文本框中。
8. 从**身份验证**下拉菜单中选择 **Azure Active Directory – 通用 MFA**。
9. 点击**连接**。

下图显示了上述步骤：

![](img/5d2bce74fd6fceebed14fd1488bf7088_779_0.png)

图15.27：从SSMS连接到Datamart的SQL端点

通过凭据后，我们即可连接到Azure SQL数据库，并运行SQL语句。下图显示了结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_779_1.png)

图15.28：在SSMS中针对Datamart的数据库运行SQL语句

## Datamarts中的行级别安全性（RLS）

我们可以在Datamarts中实现RLS来控制用户的访问。在第11章“行级和对象级安全性”中，我们了解到RLS仅对工作区中具有**查看者**角色的用户生效。这一原则在此处同样适用。在Datamarts中，实现的RLS会自动传播到下游对象，包括自动生成的数据集和报表。有趣的是，RLS也适用于Azure SQL数据库。让我们实现一个简单的RLS场景来理解其工作原理。

业务需求是限制用户访问权限，使得北美用户只能访问其所在区域的销售数据。我们在之前创建的现有Datamart上实现该场景。让我们再次查看数据模型，以更好地理解RLS实现。如下图所示，**DimSalesTerritory**表与**FactInternetSalesLive**表之间存在**一对多**关系：

![](img/5d2bce74fd6fceebed14fd1488bf7088_780_0.png)

图15.29：DimSalesTerritory与FactInternetSalesLive之间的一对多关系

因此，对**DimSalesTerritory**应用RLS会过滤**FactInternetSalesLive**表。下图显示了**DimSalesTerritory**的数据：

![](img/5d2bce74fd6fceebed14fd1488bf7088_781_0.png)

图15.30：DimSalesTerritory的数据

如上图所示，通过对SalesTerritoryGroup列应用RLS即可实现目标。

在Datamarts中实现RLS可以通过Datamart编辑器中的“数据”或“模型”视图完成。请按照以下步骤实现该场景：

1. 从“主页”选项卡点击“管理角色”按钮。
2. 点击“角色”部分下的“新建”按钮。
3. 将新角色命名为“North Americans”。
4. 选择DimSalesTerritory表。
5. 点击“筛选数据”部分下的“添加”按钮。
6. 从下拉菜单中选择SalesTerritoryGroup列，使用“等于”运算符，并在筛选条件中输入“North America”。
7. 点击“保存”。

下图显示了上述步骤：

