

每个日志条目包含五列，分别是：

- **User**：显示执行该操作的用户用户名。即使用户后来被删除，用户名仍保存在日志中。
- **Item ID**：显示受影响项目的数据库 ID。请注意，项目 ID 仅对下一列中显示的 **Item Name** 唯一，而非在整个数据库中唯一，因此同一个项目 ID 可能会重复出现。
- **Item Name**：显示受影响项目的名称。
- **Action**：描述对项目所做的更改类型。
- **Date**：保存更改所做的日期和时间。

如果您有新手用户可以访问管理控制台，那么您可能希望保留所有版本的內容，以便在出现问题时可以恢复它们。CMS Made Simple 的标准安装中没有此功能，但有一个不错的第三方模块可以额外安装。

## 归档更改并恢复它们

一旦安装了 **Archiver** 模块，它就会保存每个被编辑或删除的内容页面、全局内容块、样式表或模板的副本。它还提供了将归档中的任何内容恢复到当前状态的可能性。这意味着您的用户可以编辑网站的各个方面，而您可以稍后撤销他们的更改。

要安装 **Archiver** 模块，请打开管理控制台，然后点击 **Extensions | Module Manager**。点击 **Available Modules** 选项卡，然后点击与模块名称同一行中的 **Download & Install** 链接。模块即已安装。

安装后，模块将自动开始将更改保存到归档中。例如，如果您想跟踪首页的更改，请打开该页面并在不做任何更改的情况下保存它，以便创建该页面的第一个版本。此版本将用作未来恢复的基础。

### 行动时间 – 恢复编辑者所做的更改

假设您已按上述说明安装了 Archiver 模块，您可以按如下方式恢复编辑者所做的更改：

1. 以编辑者身份登录并编辑您有权编辑的任何页面。
2. 注销然后再次使用管理员账户登录。
3. 点击 Content | Archive Manager Module。
4. 搜索步骤 1 中已编辑的页面的名称，然后点击它。
5. 在 Expanded Archive View 中，找到该页面的倒数第二个版本（修订版）并点击 Operation 列中的最后一个图标。
6. 查看该页面。

### 发生了什么？

您已将内容的旧版本恢复并使其成为当前版本。编辑者的版本并未被删除，只是被替换，因此您可以随时将其恢复。

您可以按每个项目的存储版本数量自动清理归档（即仅保留每个页面、全局内容块、样式表或模板的最后五个版本）或按日期清理（即仅保留最近七天的版本）。要设置自动清理，请在模块的管理区域中点击 Archive Preferences 链接（Content | Archive Manager Module）。

您不必清理归档，也可以一直累积所有更改。但是，如果您每天有大量更改，则必须清理归档以防止数据库溢出。

> 该模块不能替代常规的异地数据库备份。数据仍存储在同一数据库中，数据库崩溃可能会使归档变得无用。请阅读第 11 章《Administration and Troubleshooting》中的相应部分，了解如何创建数据库备份。

## 所有默认权限的概述

权限始终授予组，并由分配给该组的所有用户共享。在下表中，第一列是权限名称，第二列是对该权限可授予角色的建议，第三列建议该权限生效的一些前提条件。

| Permission | Suggestion for the role | Description |
|---|---|---|
| Add Global Content Blocks | Editor | 该组允许创建新的全局内容块（Content \| Global Content Blocks）。取决于权限 Modify Global Content Blocks。 |
| Add Groups | Admin | 该组允许创建用户组（Users & Groups \| Groups）。取决于权限 Modify Groups。 |
| Add Pages | Editor | 该组允许在用户是所有者或额外编辑的页面下创建子内容页面。用户必须被设为页面的所有者或额外编辑。 |
| Add Stylesheet Associations | Designer | 该组允许将样式表与模板关联。 |
| Add Stylesheets | Designer | 该组允许创建新的样式表。 |
| Add Templates | Designer | 该组允许创建新的模板。 |
| Add Users | Admin | 该组允许创建具有管理控制台访问权限的新用户。 |
| Allow usage of advanced profile in TinyMCE | Editor | 该组中的用户将自动获得为 TinyMCE 模块制作的高级配置文件设置。但是，他们无法更改配置文件本身。 |
| Approve News For Frontend Display | Editor | 允许将新闻项的状态字段从 Draft 更改为 Published。该组中的用户也可以编辑新闻文章。 |
| Clear Admin Log | Admin | 清除 Site Admin \| Admin Log 中的历史记录。取决于权限 Modify Site Preferences。 |
| Delete News Articles | Editor | 该组允许删除新闻文章。取决于权限 Modify News。 |
| Manage All Content | Editor | 该组允许创建、移动和删除页面。该组允许在编辑页面时访问 Options 选项卡并更改该选项卡上可以找到的任何内容，与权限 Modify Any Page 相比。 |
| Manage Menu | Designer | 该组可以修改菜单模板并创建新的菜单模板。 |
| **Manage Themes** | Designer | 该组允许导出主题。如果存在依赖权限，用户也可以导入主题。取决于权限 **Add Stylesheet Associations**、**Add Stylesheets** 和 **Add Templates**。 |
| **Modify Any Page** | Editor | 该组允许编辑内容页面。编辑内容时没有 **Options** 选项卡。但是，一些来自 **Options** 选项卡的字段可以从 **Site Admin \| Global Settings**（选项卡 **Advanced Setup**，字段 **Basic Properties**）中添加。 |
| **Modify Events** | Admin | 该组允许添加和重新排序事件（请参阅第 8 章）。 |
| **Modify Files** | Editor | 允许使用 **File and Image Manager**，从而完全访问安装的 `/uploads` 文件夹。 |
| **Modify Global Content Blocks** | Editor | 允许编辑和删除全局内容块。 |
| **Modify Group Assignments** | Admin | 允许将用户分配到不同的组。 |
| **Modify Groups** | Admin | 允许更改组名。 |
| **Modify Modules** | Admin | 允许管理 **Extensions \| Modules** 和 **Extensions \| Module Manager**，从而安装、升级、卸载和删除模块。 |
| **Modify News** | Editor | 允许编辑新闻并创建状态为 **Draft** 的新文章。 |
| **Modify Permissions for Groups** | Admin | 允许修改组的权限。 |
| **Modify Site Preferences** | Admin | 这是一组多个权限，包括不同模块中的 **Preferences**、**Settings** 或 **Options** 选项卡。例如，用户能够访问 **News** 模块的 **Options** 选项卡、**File Manager**、**CMSMailer** 中的 **Settings** 选项卡、**Search** 模块中的 **Options** 选项卡等。 |
| **Modify Stylesheet Associations** | Designer | 允许将样式表附加到模板并解决附加关系。 |
| **Modify Stylesheets** | Designer | 允许编辑现有样式表。 |
| **Modify Templates** | Designer | 允许编辑现有模板，包括 **News**、**Search** 和 **Printing** 模块中的模板。 |
| **Modify User-defined Tags** | Admin | 允许向网站添加用户定义标签（请参阅第 10 章）。 |

