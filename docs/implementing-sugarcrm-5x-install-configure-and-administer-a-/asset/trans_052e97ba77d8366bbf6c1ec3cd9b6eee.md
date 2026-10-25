

## 现在应该出现类似下面的屏幕：

![](img/7ccaf60360d7c8e809d336edbba2263d_161_0.png)

接下来，我们需要单击 **Choose File** 按钮。这样将允许您选择要安装的模块对应的安装文件。导航到先前下载的 zip 文件所在位置并选择它。继续单击 **Upload** 按钮。

单击 **Upload** 按钮将把 zip 文件复制到 SugarCRM 服务器并为其安装做好准备。这样做后，您会注意到模块已添加到 Module Loader 工具的底部区域。要完成安装，请单击相应条目旁边的 **Install** 按钮。然后您需要接受许可证并继续完成安装向导的其余步骤。

成功安装模块后，单击 **Back to Module Loader** 按钮。请注意，模块条目已从底部列表移至顶部，所有已安装的模块和增强功能都显示在此处。如果您决定稍后禁用或卸载某个模块，只需单击列表中其条目旁边的 **Disable** 或 **Uninstall** 按钮即可。

既然我们已安装 Enhanced Search 模块，让我们更仔细地了解一下它的功能。

## Enhanced search

迄今为止我们所考察的所有工具都有一个共同点；它们都为 SugarCRM 提供了新功能。然而，之前的任何工具都不修改现有功能的行为。Enhanced Search 功能将是我们首次涉足这一领域。

如前所述，SugarCRM 中可用的搜索功能是定位系统中数据的非常有用的方式，但同时其功能也相当有限。用户遇到的一个常见问题是，搜索行为无法从默认的 AND 连接器更改为 OR。无法做到这一点会带来许多挑战，如果搜索条件需要搜索多个字段，这些挑战会更加严重。

例如，如果您希望搜索办公电话号码以 310 开头或州字段设置为 California 的联系人，您会发现默认搜索功能只会返回办公电话号码以 310 开头且位于 California 的记录；这不是您要找的记录集。Enhanced Search 修改了现有功能以消除这一限制，并且总体上以多种方式提供更丰富的搜索体验。

安装通过 Module Loader 执行，当安装完成其功能后，它立即对所有 SugarCRM 用户可用。安装后，列表视图屏幕上的 Advanced Search 面板应类似于以下内容：

![](img/7ccaf60360d7c8e809d336edbba2263d_162_0.png)

请注意显示字段后面紧跟的向下图标。单击该图标允许您选择与搜索相关的各种选项，包括以下内容：

- OR
- AND
- Operator (LIKE, RLIKE, EQUALS, IN, and BETWEEN)

这些选项允许您通过将默认连接器 AND 更改为 OR 来操纵搜索功能的行为，以便执行搜索，其中所需结果满足正在搜索的各种字段中的一个或多个字段的指定条件，而不是所有字段。LIKE 和 RLIKE 运算符允许您在不知道确切值的情况下搜索值的一部分。例如，如果您希望获取街道地址包含 **Main Street** 的所有联系人列表，无论门牌号如何，使用 LIKE 并使用以下搜索值将为您提供这些结果：% **Main Street** %。

此场景还让我们了解 RLIKE 选项的用法。与 LIKE 选项类似，它也用于搜索值的一部分，但它允许您在同一字段中查找多个值中的一个。例如，我们可以使用此选项搜索地址在 **Main Street** 或 **Broadway Avenue** 的联系人，方法是使用 RLIKE 和以下搜索值：**Main Street | Broadway Avenue**。

与 RLIKE 运算符类似的是 IN 运算符，它允许您通过以下方式指定值来搜索同一字段中的多个值：'value 1', 'value 2', 'value 3'。区别在于，使用 IN 会导致 SugarCRM 搜索在指定字段中与任何指定值精确匹配的记录，而 RLIKE 只需要字段包含该值，而不考虑字段中任何其他信息。

最后，使用 BETWEEN 选项允许您通过定义日期范围来搜索日期字段。这是 SugarCRM 默认不提供的常见搜索请求。

对于普通用户，例如 RayDoc 的用户，能够使用多种方法搜索数据非常强大且必不可少。

## Google connectors

可在 `SugarForge.org` 上免费获得，这些工具将允许您将 SugarCRM 系统中的数据同步到 Google 日历和/或 Google 联系人（取决于您选择安装的连接器）。

此增强功能解决的主要业务需求是将 CRM 数据扩展或暴露给非 SugarCRM 用户或系统。通过这些连接器，您将能够在 Google 日历中查看最初在 SugarCRM 中安排的活动。一旦在 Google 日历中，您可以像使用其他活动一样使用这些活动，包括与他人共享，或使用 Google Sync 将其同步到移动设备。

同样，联系人连接器允许您使用 SugarCRM 中的信息填充 Google 联系人列表。这又提供了一种扩展 SugarCRM 数据的方法，与 Google 日历场景类似，Google Sync 可用于将数据同步到移动设备。

即使是 RayDoc 这样的组织也能从这种功能中受益。例如，将 SugarCRM 数据同步到 Google 日历将允许他们与客户共享日历，让客户在不先联系 RayDoc 的情况下查找方便的预约时间。RayDoc 的服务人员还可以使用 Google Sync 配合 Google 联系人连接器，在其 BlackBerry 设备上维护客户姓名列表。这样，即使他们没有互联网访问权限，或身处无线服务不可行的区域，也能查看宝贵的联系人信息。当然，如果您使用的是 Android 智能手机，过程会更加无缝，因为手机与 Google 之间的同步是内置的，不需要 Google Sync 组件。

安装后，您需要配置连接器以将其链接到特定的 Google 帐户。您可以使用个人 Google 帐户或与企业帐户关联的帐户。如果您还没有 Google 帐户，则需要先注册一个才能继续。

掌握 Google 帐户信息后，并假设您已安装其中一个连接器，请单击 SugarCRM 中的 **My Account** 链接，然后单击左侧快捷菜单上的 **Google Account**。应出现以下屏幕：

![](img/7ccaf60360d7c8e809d336edbba2263d_165_0.png)

通过单击 **Edit** 然后 **Save** 调整设置。请注意，设置特定于每个 SugarCRM 用户。这允许 RayDoc 的用户从该功能中受益以启用它，而不希望使用它的用户可以简单地忽略它，而不会影响任何其他用户。

配置后，数据将在 SugarCRM 和 Google 之间自动同步。

## Microsoft Outlook connector

本书的这一部分，以及随后讨论 Microsoft Office 集成的部分，仅面向 Microsoft Windows 用户。如果您将用于访问 SugarCRM 安装的工作站未使用 Windows，则可能希望跳过此部分。

SugarCRM 的内置电子邮件客户端适用于许多组织，但与 Microsoft Outlook 等功能齐全的电子邮件客户端应用程序相比，功能有限。对于某些组织，功能差异过大，他们最好继续使用 Outlook。

然而，挑战在于，这样做意味着您与客户或潜在客户之间的电子邮件通信不会自动链接到 SugarCRM 中对应的记录，也不会供其他用户日后参考。

为解决此问题，已在 SugarCRM 和 Outlook 之间开发了多个连接器。它们都提供类似的功能；它们在 Microsoft Outlook 中提供功能，允许 Outlook 用户将日历项目、联系人和电子邮件与 SugarCRM 同步。

我们将更仔细地了解 **GrinMark** Microsoft Office 外接程序 for SugarCRM 解决方案，因为它还包含允许我们将 SugarCRM 与 Microsoft Word 集成的功能。我们将在下一节中更详细地研究这一点。但是，欢迎探索 SugarForge.org 上可用的其他选项，例如 **KINAMU** Outlook Connector，因为它们提供类似的功能。

要开始研究 GrinMark 外接程序，让我们先快速了解其安装过程，因为它不利用 SugarCRM Module Loader 功能。安装过程通过双击 GrinMark 外接程序安装程序文件 (`GrinMarkOfficeSetup.msi`) 启动，就像处理其他基于 Windows 的程序一样。

