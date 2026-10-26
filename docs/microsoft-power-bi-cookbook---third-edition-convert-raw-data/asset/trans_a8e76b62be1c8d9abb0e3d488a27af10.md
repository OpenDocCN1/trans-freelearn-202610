

6. 安装完成后，您将看到与图 1.13 类似的屏幕：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_42_0.png)

图 1.13：SQL Server 成功安装

7. 单击关闭按钮。

## 安装 SQL Server Management Studio

- 1. 运行 SSMS-Setup-ENU.exe 文件。
- 2. 在安装启动屏幕上，单击安装按钮。
- 3. 如果收到用户账户控制提示，请选择是。
- 4. 如果安装失败，请重新启动计算机，然后再次运行 SSMS-Setup-ENU.exe 文件。
- 5. 安装完成后，单击重启按钮以重新启动计算机并完成安装。

Microsoft SQL Server 以及 SQL Server Management Studio 现已安装完成并可供使用。

## 如何安装 AdventureWorks 数据库

AdventureWorks 数据库是由 Microsoft 提供的一个示例数据库，常用于演示和学习目的。该数据库旨在展示 Microsoft SQL Server 的功能和特性。AdventureWorks 数据库已随着不同版本的 SQL Server 而演进，截至本书出版时最新版本为 2022。

该数据库代表了一个名为 AdventureWorks Cycles 的虚构公司，这是一家生产和销售自行车及相关产品的制造公司。AdventureWorks 数据库包含各种表，用于模拟公司运营的不同方面，例如客户、销售、产品、员工等。

AdventureWorks 数据库的每个版本都包含三种不同的备份（.bak）文件：

- - **联机事务处理（OLTP）**：完整数据集
- - **数据仓库（DW）**：用于报告目的的数据集汇总版本
- - **精简版（LT）**：OLTP 数据集的精简版本

就我们的目的而言，我们将使用 AdventureWorks 数据库的数据仓库（DW）版本。

要安装和配置 AdventureWorks DW 数据库，请按照以下步骤操作：

- 1. 使用以下链接从 GitHub 下载备份（.bak）文件：https://bit.ly/3QTnCXI。下载的文件名为 AdventureWorksDW2022.bak。
- 2. 打开 Windows 文件资源管理器，将 AdventureWorksDW2022.bak 文件从下载目录移动到 C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup 目录。
- 3. 如果尚未打开，请打开 SQL Server Management Studio 并连接到本地 SQL Server 实例，如本食谱的“工作原理”部分所述。
- 4. 在对象资源管理器中展开 SQL Server 实例，然后右键单击数据库并选择还原数据库...
- 5. 在常规页面上，选择设备，然后选择省略号（...）。在选择备份设备窗口中，单击添加按钮。选择 AdventureWorksDW2022.bak 文件并单击确定按钮。单击选择备份设备窗口中的确定按钮，最后单击常规页面上的确定按钮。请使用图 1.14 作为按顺序执行这些步骤的指导。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_43_0.png)

图 1.14：还原 AdventureWorksDW2022 备份文件

在 **连接到服务器** 窗口中，确保 **服务器类型** 选择为 **数据库引擎**，**服务器名称** 显示为本地计算机的名称，并且 **身份验证** 设置为 **Windows 身份验证**。单击 **连接** 按钮以连接到本地 SQL Server 实例，如图 1.18 所示：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_46_0.png)

图 1.18：SQL Server Management Studio

## 另请参阅

有关本食谱中涵盖主题的更多信息，请参阅以下链接：

- - Microsoft SQL Server 文档：https://bit.ly/3FVerji
- - SQL Server Management Studio 文档：https://bit.ly/47x3nV8
- - AdventureWorks 示例数据库：https://bit.ly/3Ue8y8J

## 安装和使用 Power BI Report Builder

Power BI Report Builder 是 Microsoft Power BI 软件套件的一部分的独立免费报表制作工具。Power BI Report Builder 是久负盛名的 Report Builder 工具的演进版，它允许用户创建分页报表（也称为传统 SQL Server Reporting Services 或 SSRS 报表）。在 Power BI Report Builder 中创建的报表可以发布到 Power BI 服务中并在其中查看，与传统的 Power BI Desktop 报表类似。

## 准备工作

要为本食谱做好准备，首先从以下链接下载 Power BI Report Builder 安装程序：https://www.microsoft.com/en-us/download/details.aspx?id=105942。

在浏览器中打开页面后，选择您的语言，然后单击 **下载** 按钮下载 PowerBiReportBuilder.msi 文件。

## 如何安装 Power BI Report Builder

要安装 Power BI Report Builder，请按照以下步骤操作：

- 1. 下载 PowerBiReportBuilder.msi 后，在下载窗格中单击打开文件或从下载目录运行该文件。
- 2. 在欢迎页面上，单击下一步按钮。
- 3. 接受许可条款并单击下一步按钮。
- 4. 选择目标文件夹或接受默认值并单击下一步按钮。
- 5. 单击安装按钮。
- 6. 如果收到用户账户控制提示，单击是按钮。
- 7. 安装 Power BI Report Builder 后，单击完成按钮。

Power BI Report Builder 现已安装完成并可供使用。

## 工作原理

与创建文件扩展名为 PBIX 的 Power BI Desktop 不同，Power BI Report Builder 将报表文件保存为报表定义语言（.rdl）文件。RDL 文件实际上是可扩展标记语言（XML）文件，这也是 SQL Server Reporting Services（SSRS）使用的文件格式。Power BI Report Builder 文件可以发布到 Power BI 服务或 Power BI Report Server，后者是 SQL Server Reporting Services 的继任者。

分页报表与 Power BI Desktop 报表的工作方式截然不同。分页报表针对打印进行了优化，可显示跨越多个报表页面的长表格或矩阵。这与 Power BI Desktop 只能在不滚动的情况下显示有限行数的方式不同。

另一个关键区别是，与 Power BI Desktop 报表相比，分页报表具有有限的交互功能。在 Power BI Desktop 中，单击视觉对象可以影响并改变页面上其他视觉对象的显示。分页报表之间没有这种视觉对象的交互。分页报表的交互是通过参数完成的，其功能类似于 Power BI Desktop 报表中的切片器。

分页报表与 Power BI Desktop 报表之间的最后一个关键区别是，可用的视觉对象数量有限，分页报表约有 ~12 个，而 Power BI Desktop 有数十个默认视觉对象，以及 AppSource 上提供的数百个自定义视觉对象。

要运行 Power BI Report Builder，在 Windows 搜索栏中开始输入 Power BI Report Builder，然后在 Windows 开始菜单中运行 Power BI Report Builder 应用。接下来，请按照以下步骤操作：

- 1. 在开始窗口中，选择新建报表然后选择表或矩阵向导。
- 2. 确保已选择创建数据集单选按钮，然后单击下一步按钮。
- 3. 单击新建...按钮开始创建新数据集。
- 4. 在数据源属性窗口中，单击生成...按钮。

## 另请参阅

有关本食谱中涵盖主题的更多信息，请参阅以下链接：

- - 什么是 Power BI Premium？：https://bit.ly/3QY30L
- - 什么是 Power BI 嵌入式分析？：https://bit.ly/3QY4fgg

## 创建本地数据网关

Microsoft 本地数据网关（或简称为网关）是在本地环境中运行或作为在云中运行的基础设施即服务（IaaS）资源运行的 Windows 服务。网关的唯一目的是在本地和 IaaS 数据源与 Microsoft Azure 服务（例如 Power BI、Power Apps、Power Automate、Azure Analysis Services 和 Azure Logic Apps）之间通过到 Azure Service Bus 的出站连接实现安全（加密）和高效的数据传输。需要注意的是，如果 Power BI 报表使用的所有数据源都在云中（因此不是本地的）并且可由 Power BI 服务访问，则不需要 Microsoft 本地数据网关，因为 Power BI 服务将使用原生云网关来访问原生云数据源；这可能是例如 Azure SQL 数据库或 Azure SQL 受管实例。

安装后，网关可用于安排导入的 Power BI 数据集的数据刷新，以支持使用 DirectQuery 构建的 Power BI 报表和仪表板，以及对 Analysis Services 数据库的实时连接。网关也可由 Power Apps、Azure Logic Apps、Power Automate、Azure Analysis Services 和数据流使用。

单个本地数据网关可以支持多个数据源的刷新和查询活动，并且网关的使用权限可以与多个用户共享。目前，网关通过计划导入支持所有常见数据源，包括开放数据库连接（ODBC）连接，以及通过实时连接和 DirectQuery 支持许多最常见的数据源。

## 准备工作

网关所需硬件资源取决于连接类型（例如导入与实时连接）、Power BI 服务中报表和仪表板的使用情况，以及网关处理的与本地源系统相比的数据量比例。建议从 8 核 CPU、8 GB RAM 服务器和 64 位 Windows 2012 R2 或更高版本的操作系统开始。还建议使用至少 4 GB 磁盘空间的固态硬盘。该机器不能是域控制器，并且为了保持 Power BI 内容的可用性，网关服务器应始终开启并连接到互联网。

网关的另一个重要考虑因素是网关服务器相对于 Power BI 租户和由网关支持的数据源的位置。例如，如果 SQL Server 数据库是主要网关源并运行在美国西部的一台服务器上，并且组织的 Power BI 租户位于美国西部 Azure 区域，则网关应安装在美国西部 Azure 区域的服务器或潜在的 Azure 虚拟机（VM）上，或美国西部的位置。

