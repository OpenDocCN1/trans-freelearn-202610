

## SugarCRM 快速导览

成功登录系统后，将显示 **Home** 选项卡，如下图所示。请注意，本概述假设您使用具有管理员权限的帐户登录了系统。屏幕布局的各个关键元素已按如下方式突出显示：

![](img/7ccaf60360d7c8e809d336edbba2263d_99_0.png)

- **Navigation tabs**：默认情况下，它们按功能组织。单击主标题可选择模块，例如 **Contacts** 或 **Accounts**。

- **System links**：**My Account**、**Employees**、**Logout** 和 **About**。**Admin** 链接仅可供分配了管理权限的用户使用。

- **Search box**：它有助于在 SugarCRM 保存的关键数据中搜索文本字符串。

- **Last viewed**：它是您最近查看过的记录的有用轨迹。

- **Navigation shortcuts box**：这些是在每个模块中有用的特定快捷方式。

- **Quick new item box**：它是一个快速数据输入框，用于为当前模块创建新项目。

- **Main screen body**：在 **Home** 选项卡上，这包括 **Cases**、**Opportunities**、**Appointments**、**Leads**、**Tasks** 和销售管道图。还可以使用 **Add Sugar Dashlets** 按钮添加其他项目。

**Home** 选项卡的屏幕布局与所有其他选项卡使用相同的布局。在选项卡之间移动时，快捷方式列表中可用的快捷方式会根据特定选项卡发生变化。主屏幕正文显示由 _选项卡名称_ 指示的信息。**Quick New Item** 案例会更改，以根据选项卡提供创建新项目的快速访问，例如 **New Contact**、**New Account**、**New Opportunity** 等等。

## 主题

前面我们提到，您可以选择备用主题来更改与 SugarCRM 使用相关的外观方面。在继续之前，让我们快速看一下。在屏幕的 **System Links** 区域（右上角）中，单击 **Page Styles** 链接，并将主题值更改为 **Links**。更改主题值会导致 SugarCRM 自动应用新设置。

请注意，更改主题会导致 SugarCRM 屏幕的许多方面发生变化。导航选项卡不再是选项卡，而是转换为侧边菜单系统。基本上，快捷方式与选项卡的位置已互换。此外，配色方案和图形也非常不同。

无论使用哪个主题，都会显示相同的 CRM 信息。只是布局不同。选择您喜欢的主题，或不时更改它，以在使用 CRM 系统时保持一些乐趣。如果您有兴趣自定义现有主题，或者也许创建自己的主题以匹配您公司的品牌，您将在 _Chapter 5_ 中找到所需信息。

> 在本章的其余部分，我们将假设您在安装 SugarCRM 时，还选择安装了示例数据。（有关安装 SugarCRM 的附录中 _Database Configuration_ 部分的更多信息，请参阅。）稍后，我们将快速重新安装以删除演示数据，然后导入您自己组织的数据。

## SugarCRM 基础：数据关系

在深入了解应用程序的特定功能领域之前，了解系统内部关系和层次结构的某些方面非常重要。

在本书中，我们一直提到 _Accounts_、_Contacts_、_Opportunities_ 和其他构成 CRM 系统的数据实体。我们还讨论过，CRM 系统的一些主要目标是存储此类信息，并轻松演示它们之间的关系。正是这种理念，使 CRM 系统不仅能为您提供有关待处理销售的信息，还能提供与之相关的客户信息。为了实现这一点，CRM 系统中必须存在规定数据实体如何相互关联或连接的规则。为了更好地了解 SugarCRM，我们必须首先检查这些有助于构建其层次结构的规则。

在 SugarCRM 的世界中，Account 是顶级实体。默认情况下，所有其他数据项（包括 Contacts、Opportunities、Cases 等）都可以轻松链接到 Account。您还可以将任意数量的这些项链接到任何给定的 Account。此功能允许将多个联系人链接到单个帐户，通常称为一对多关系。

以下插图描绘了层次结构：

![](img/7ccaf60360d7c8e809d336edbba2263d_101_0.png)

虽然低级数据实体可以轻松链接到 Accounts，但它们在彼此之间链接的方式上受到限制。例如，Case 默认情况下期望链接到 Account。只有在首先链接到 Account 之后，才能将其与 Contact 关联。此示例突出了一个可能需要自定义的区域，具体取决于您的业务使用的是 B2B 还是 B2C 模式。

此外，有些关系只能以单一方式建立。一个很好的例子是 Task 和 E-mail 之间的关系。可以在两者之间建立关系，但要做到这一点，您必须访问相关的电子邮件并在那里建立关系，因为尝试反向操作是不允许的。如果您尝试通过访问 Task 来做到这一点，您会觉得无法建立该关系。

关于低级项目之间关系的最后一个重要点是，它们通常表示一对一关系。刚才解释的 Task 和 E-mail 示例很好地突出了这一点，因为只有一个 Task 可以链接到任何给定的 E-mail。

## SugarCRM 导航：帐户和联系人

那么——我们来了——SugarCRM！

让我们更熟悉一下使用系统。首先，（我将假设您从现在起已切换回正常的 Sugar Theme）尝试访问几个模块，方法是先将鼠标悬停在组标题选项卡上，然后选择显示的模块名称之一。例如，将鼠标悬停在 Marketing 上，然后选择 **Accounts** 以访问帐户模块，如下图所示：

![](img/7ccaf60360d7c8e809d336edbba2263d_102_0.png)

不同的信息集组织在不同的模块中，每个模块都有一个选项卡，您单击它以访问和使用它。

SugarCRM Community Edition 5.5 中的模块按以下方式分组：

| Group header tab name | Modules |
|---|---|
| Home | Home, Dashboard |
| Sales | Accounts, Opportunities, Leads |
| Marketing | Campaigns, Contacts, Accounts, Leads |
| Support | Cases, Accounts, Contacts |
| Activities | Activities, Calendar, E-mails, Calls, Meetings, Tasks, Notes |
| Collaboration | E-mails, Documents |
| Reports | Dashboard |

SugarCRM 的 Professional 和 Enterprise Editions 扩展了此列表，一些额外项目为产品、报价、预测和报告以及团队销售和工作流功能提供模块。

系统管理员可以控制特定用户可以访问哪些模块，在某些情况下，用户也可以隐藏或重新排列选项卡。用户还可以选择不使用分组，而是使用代表每个模块的选项卡，具体取决于其导航偏好。如果您不使用组选项卡方法，您会发现 SugarCRM 只能在导航区域显示 12 个选项卡。其他模块可以通过可见最后一个选项卡右侧的溢出区域访问。使用在左侧像菜单系统一样列出模块的主题（例如 Links 或 Pipeline）消除了对溢出区域的需求，因为用户可访问的所有模块都会自动显示。

## 列表和详细信息视图屏幕

将鼠标悬停在 Sales 上并单击 Accounts 选项卡。这将显示系统中所有帐户的列表。在 SugarCRM 中显示列表的屏幕称为列表视图。此屏幕是 Accounts 列表视图。如果您安装了演示数据，您的屏幕应该类似于以下内容：

![](img/7ccaf60360d7c8e809d336edbba2263d_103_0.png)

让我们想象一下，我们正在查看 RayDoc 的帐户列表——RayDoc 组织的一组示例帐户、联系人、线索、机会等。显然，公司名称是编造的，因为我们不能在此处公开真实公司的信息，但这里的示例数据是为了演示系统所需的所有关键功能，以使您的 CRM 部署取得真正的成功。

