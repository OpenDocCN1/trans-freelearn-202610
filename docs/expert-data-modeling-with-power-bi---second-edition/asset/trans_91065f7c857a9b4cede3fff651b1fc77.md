

![](img/5d2bce74fd6fceebed14fd1488bf7088_743_0.png)

图 14.15：已将 Reseller Sales 数据添加到 Power BI 数据集

因此，新的数据模型包含来自远程数据集的表以及来自数据仓库的两个导入表。Power BI 服务中的远程数据集已实施 RLS，因此我们期望 RLS 会传播到新的数据模型。新的报表文件为 `Chapter 14, Composite Model Internet Sales.pbix`，可从此处下载：

[https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2014/Chapter%2014,%20Composite%20Model%20Internet%20Sales.pbix](https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2014/Chapter%2014,%20Composite%20Model%20Internet%20Sales.pbix)

如 *第 11 章、行级和对象级安全* 中所述，RLS 仅在用户对包含的工作区具有 **Viewer** 角色或报表通过链接、直接访问或应用与用户共享时才会生效。因此，我将新报表发布到 Power BI 服务，并在工作区上为期望的安全组设置了 **Viewer** 角色。以下图像显示了发布到 Power BI 服务的新报表的血缘视图：

![](img/5d2bce74fd6fceebed14fd1488bf7088_744_0.png)

图 14.16：复合模型的血缘视图

如前图所示，复合模型的链长为 2。以下图像显示了当安全组成员用户登录 Power BI 并运行报表时会发生什么：

![](img/5d2bce74fd6fceebed14fd1488bf7088_744_1.png)

图 14.17：复合模型 Internet Sales 报表

如前图所示，对第一链实施的 RLS 已生效并识别出 Rachel 登录，且在第一链数据集中的 **Internet Sales** 度量上也按预期工作。但在 **Reseller Sales** 上发生了奇怪的事情，进而影响了 **Total Sales**，即 `[Internet Sales] + [Reseller Sales]`。虽然唯一的 **SalesTerritoryRegion** 是 **Germany**，这是正确的，但总数完全错误。事实上，Rachel 看到了所有 **Reseller Sales** 而不是 **Germany** 的销售。原因是 **DimEmployee** 与 **FactResellerSales** 表之间的关系是受限关系，如 *图 14.16* 所示。

事实上，DirectQuery 表与导入表之间的所有关系都是受限关系。我们在 *第 9 章、星型架构和数据建模通用最佳实践* 中 *处理多对多关系* 部分触及了受限关系，在那里我们讨论了两种关系评估类型：常规和受限。简要回顾一下，当两个表具有相同的存储模式且两者之间的关系不是多对多时，两个表之间的关系是常规的。如果违反上述任一条件，表之间的关系就是受限的。

当关系受限时，**表扩展** 不会发生。表扩展发生在 **多对一** 关系 **多** 侧的表或具有 **一对一** 关系的表上。表扩展意味着 **一** 侧表的列可由 **多** 侧表访问。例如，查看 *图 14.16*；你可以想象一个宽的 **FactResellerSales**，它还包含 **DimReseller** 表的所有列。这就像使用 `RELATED()` 函数从 **FactResellerSales** 表访问 **DimReseller** 列一样。

这就是为什么当关系受限时，`RELATED()` 函数不受支持。当关系受限时，从 **一** 侧到 **多** 侧的一对多关系的筛选传播不会按预期工作。因此，当 Rachel 打开报表时，她会看到所有经销商销售，而不仅仅是她自己的。为了修复此问题，我们在 **FactResellerSales** 表上添加了另一个 RLS 规则，使用以下 DAX 表达式对 **SalesTerritoryKey** 列强制执行筛选：

```
VAR __SalesReps =
CALCULATETABLE(
    VALUES(DimEmployee[EmployeeKey]),
    TREATAS( {USERPRINCIPALNAME()}, 
DimEmployee[EmailAddress] )
)
RETURN
FactResellerSales[EmployeeKey] in __SalesReps
```

在具有 Power BI 数据集或 Azure Analysis Services 的 DirectQuery 的复合模型中，远程表不可用于创建 RLS 规则。但我们仍可运行 DAX 表达式来筛选导入表。例如，如下图所示，DimEmployee（远程表）在管理角色窗口中不可用。但我们仍可通过在远程表上运行 DAX 表达式来筛选 FactInternetSales：

![](img/5d2bce74fd6fceebed14fd1488bf7088_746_0.png)

图 14.18：在复合模型中创建新的 RLS 角色

下图显示了在向复合模型添加新的 RLS 角色后 Rachel 的报表结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_746_1.png)

图 14.19：Rachel 运行的带 RLS 的复合模型

当 Pamela 打开同一报表时，她只能看到她自己的数据，这正是我们期望报表的工作方式。下图显示了 Pamela 运行的报表：

![](img/5d2bce74fd6fceebed14fd1488bf7088_747_0.png)

图 14.20：Pamela 运行的带 RLS 的复合模型

正如我们所见，受限关系使得在具有 Power BI 数据集 DirectQuery 的复合模型中 RLS 的工作方式与常规模型略有不同。话虽如此，我们了解到应用于不同数据集链的 RSL 总是向下流，这意味着链 1 数据集中的 RLS 会筛选链 2 数据集中的数据。这会影响我们从权限角度在数据集上实现进一步报表的方式。例如，考虑这样一种情况：我们希望启用贡献者使用具有 DirectQuery for Power BI 数据集的复合模型数据集结构创建报表。我们将在下一节讨论这个问题。

## 为贡献者（报表作者）设置数据集权限

众所周知，对数据集实施的 RLS 仅对通过共享链接访问报表、具有报表只读直接访问权限或在工作区具有 Viewer 角色的用户生效。报表作者应具有对引用数据集的 Build 权限，以便在 Power BI Desktop 中连接到数据集以创建精简报表。精简报表是我们基于现有数据集构建的报表。换句话说，精简报表没有数据模型，只有数据可视化。导航到期望的工作区后，按以下步骤向贡献者授予 **Build** 权限：

- 1. 将鼠标悬停在第一链数据集上并单击 **更多选项** 省略号按钮。
- 2. 单击 **管理权限** 选项。
- 3. 将鼠标悬停在用户帐户或安全组上并单击 **更多选项** 省略号按钮。
- 4. 单击 **添加构建。**

下图显示了上述步骤：

![](img/5d2bce74fd6fceebed14fd1488bf7088_748_0.png)

图 14.21：为贡献者授予链式数据集的构建权限

虽然我们在本章中实施的复合模型场景并不过于复杂，但它们是现实世界的场景，可以帮助我们解决更具挑战性的实施。该功能于 2023 年 4 月刚刚普遍可用，其功能将随着时间的推移而改进。因此，我强烈建议关注 Microsoft 官方网站以监控更改、限制和注意事项。以下是 Microsoft 的官方文档：https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-directquery-datasets-azure-analysis-services?WT.mc_id=DP-MVP-5003466。

## 总结

在本章中，我们了解了 Power BI 中复合模型的新术语，以及如何在 Power BI Desktop 和 Power BI 服务上启用对 Power BI 数据集的 DirectQuery 连接，并实施了简单但实用的场景，以为未来的实施和更复杂的场景建立坚实的基础。在下一章中，我们将了解 Power BI 中数据建模的其他新功能和能力。

### 加入我们 Discord！

加入 Packt Discord 服务器上的大数据与分析社区！

与 558 名其他成员一起聊天，享受免费的语音和文字聊天。

[https://packt.link/jps2H](https://packt.link/jps2H)

![](img/5d2bce74fd6fceebed14fd1488bf7088_749_0.png)

