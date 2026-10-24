

Figure 2.5 – 适用于 macOS 的 Excel AutoRecover 首选项

鉴于 AutoRecover 的功能有限，你大概率会想要探索将 AutoSave 与 OneDrive 配合使用，以便在恢复文档的先前版本方面拥有更多选择。

## 将 AutoSave 与 OneDrive 配合使用

旧版 Excel 曾经内置 AutoSave 命令，可定期备份 Excel 工作簿。该功能在 Excel 2007 的大规模改版中被搁置。幸运的是，对于保存到 Microsoft OneDrive 和 SharePoint Online 服务的文档，AutoSave 已回归。如果你拥有 Microsoft 365 订阅，那么你已经拥有一个 OneDrive 账户。除此之外，任何人都可以通过创建免费的 Microsoft 账户获得 5 GB 的免费存储空间。如果你需要更多空间，家庭用户可以以每年 19.99 美元购买 100 GB 的存储空间，而 OneDrive for Business 起价为每用户每月 5 美元。了解更多信息或订阅，请访问 www.onedrive.com。

通常，保存到 OneDrive 的文件会每隔几秒钟保存一次，这意味着你丢失工作的可能性要小得多。如图 2.6 所示，你可以通过快速访问工具栏中的滑块，按工作簿逐个开启或关闭 AutoSave：

![](img/0c7a357df7e57e06b6d9f631c219040c_59_1.png)

Figure 2.6 – AutoSave 切换开关

## 将文件保存到 OneDrive

如果你为给定文档开启了 AutoSave，系统会提示你将文件上传到 OneDrive。如图 2.7 所示，你将选择要使用的平台，然后系统会提示你为文件命名：

![](img/0c7a357df7e57e06b6d9f631c219040c_60_0.png)

Excel 的标题栏始终显示工作簿的名称。如果你将文档保存到 OneDrive 或 SharePoint Online，通常，你会在文件名后看到“Saving”或“Saved”字样。“Saving”表示你对电子表格所做的更改尚未保存。“Saved”表示已保存包含你更改的新版本。Saving/Saved 指示器仅适用于保存到 Microsoft Cloud 的文件。有时，还会出现其他消息，例如“Awaiting Upload”或“Last Modified”。你仍然可以按 Ctrl + S（⌘ + S）。或者，如果你想特别确保工作已保存，可以点击 AutoSave 旁边的保存图标。

我刚才讨论的上传方法不允许你选择保存文件的文件夹，因此你的工作簿将出现在 **OneDrive** 账户的根目录级别。幸运的是，当你使用此方法时，可以完全控制：

1. 选择 File | Save As。
2. 选择 **OneDrive** 或 **SharePoint Online**，如 **Figure 2.8** 所示：

![](img/0c7a357df7e57e06b6d9f631c219040c_61_0.png)

Figure 2.8 – 将 Save As 与 OneDrive 配合使用

3. 点击 **More options** 以显示 **Save As** 对话框。
4. 为你的电子表格选择或创建一个文件夹，指定名称，然后点击 **Save**。

但是，假设你在 **Figure 2.7** 中指定了文件名，但现在希望将文件重新命名或移动到其他位置。为此，请执行以下步骤：

1. 点击 Excel 屏幕顶部标题栏中的 _filename_。
2. 可选：指定新文件名，如 **Figure 2.9** 所示——这会在 **OneDrive** 中重命名文件，而不是制作新副本。
3. 可选：点击 **Location** 下方的按钮以显示一个对话框，你可以使用该对话框将文件移动到 **OneDrive** 中的不同文件夹。这与 **Save As** 对话框的工作方式类似：

![](img/0c7a357df7e57e06b6d9f631c219040c_61_1.png)

Figure 2.9 – 重命名和/或重新定位电子表格

## 访问先前的 AutoSave 版本

当你将文件保存到硬盘时，通常，你只能访问上次保存的副本。在本章后面的 _Always create backup_ 部分中，我将讨论一种你可以添加另一层备份的方法。当然，**AutoSave** 远不止于此，它允许你检索多个版本的工作。以下是如何访问 **AutoSave’s Version History** 功能：

1. 点击 Excel 屏幕顶部标题栏中的 _filename_。
2. 选择 **Version History**，如 Figure 2.9 所示。
3. **Version History** 任务窗格将出现在 Excel 的右侧，如 Figure 2.10 所示：

![](img/0c7a357df7e57e06b6d9f631c219040c_62_0.png)

4. 点击你想要检索的备份副本下方的 **Open Version**。
5. 如 Figure 2.11 所示，你可以点击 Excel **Message Bar** 中的 **Restore** 以用此版本替换最新的备份。或者，你可以在 **File** 菜单中选择 **Save As** 以将备份副本保存到你选择的位置：

![](img/0c7a357df7e57e06b6d9f631c219040c_63_0.png)

Figure 2.11 – Restore 按钮

## AutoSave 局限性

尽管功能名称相似，**AutoSave** 和 **AutoRecover** 的工作方式不同。如前所述，**AutoRecover** 允许你在当前工作会话期间恢复最多五个不同版本的文件。相反，**AutoSave** 会定期备份你的文件，但并非每个中间备份都可访问。**AutoSave** 最适合用于恢复文件在先前工作会话期间的状态。理想的场景是能够同时使用 **AutoSave** 和 **AutoRecover**，但 **AutoRecover** 仅适用于保存在本地计算机上的文件，而 **AutoSave** 仅适用于保存到云端的文件。

通常，**AutoSave** 将是一个设置一次即可遗忘的功能，但有时，可能会出现 **AutoSave** 无故被禁用的情况。

## 解决 AutoSave 被禁用的问题

默认情况下，任何保存到 **OneDrive**、**OneDrive for Business** 或 **SharePoint Online** 的文档都会自动启用 **AutoSave**。但是，有时，你可能会遇到 **AutoSave** 命令已被禁用的情况。以下是一些可能针对特定文档禁用 **AutoSave** 的场景：

- 你尚未保存文档，或者你将其保存到了本地硬盘或网络上。
- 有时，通过 **Microsoft Teams** 或 **SharePoint Online** 打开的工作簿，其 **AutoSave** 最初处于禁用状态。通常，你可以在打开文件后开启 **AutoSave**。
- 工作簿保存为 Excel 97–2003 工作簿格式（.xls 格式）。
- 我将在 _Chapter 3, Quick Access Toolbar Treasures_ 中讨论的旧版 **Share Workbook** 功能可能已启用。你必须禁用此功能才能使用 **AutoSave**。
- 文件具有工作簿级密码。选择 **Review** | **Protect Workbook** 并删除密码。
- 一个或多个 **Tables** 和/或 PivotTables 中启用了 **Refresh Data When Opening the File** 设置。我将在 _Chapter 12, Power Query_ 中讨论此功能，你必须禁用它才能使用 **AutoSave**。

还有更多场景可能会阻止 **AutoSave** 工作，但上述列表列出了最常见的原因。

> **Nuance**
> 为给定电子表格启用 **AutoSave** 将禁用 Excel File 菜单中的 **Save As** 命令。你可以选择 **Save As Copy** 来制作电子表格的离线副本，但请记住，你对副本所做的任何更改都不会与 **OneDrive** 或 **SharePoint Online** 同步。

当然，如果你希望控制文档保存的时间，可以关闭默认的 **AutoSave** 设置。

## 永久禁用 AutoSave

如果你愿意，可以为未来的工作簿禁用 **AutoSave** 功能：

1. 选择 **File** | **Options**。
2. 选择 **Save** 部分。
3. 取消选中 **AutoSave files stored in the Cloud by default in Excel** 的复选框，如 Figure 2.3 所示。
4. 点击 **OK**。

在 Excel for macOS 中，步骤略有不同：

1. 选择 **Excel** | **Preferences**。
2. 选择 **Save** 部分。
3. 取消选中 **Turn on AutoSave by default** 的复选框，如 Figure 2.5 所示。
4. 关闭 **Preferences** 对话框。

除非你为特定工作簿开启 **AutoSave** 命令，否则 Excel 不会再自动保存存储在 **OneDrive** 或 **SharePoint Online** 中的文档。在旧版 Excel 中，我们可以从 **Quick Access Toolbar** 部分中删除 **AutoSave** 命令：

1. 右键单击 **AutoSave** 命令。
2. 选择 **Remove from Quick Access Toolbar**。

