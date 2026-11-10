
![](img/b18244e6621ecc39cef03ff6c0e55e7c_156_0.png)

图 11.19：属性高级选项卡

Documentum 提供了众多搜索运算符，例如：

- 包含
- 以...结尾
- 大于
- 是
- 小于

## 11.3.2.7 输入掩码

这是一个重要的字段，如果使用得当，可以发挥奇效。输入掩码字段可以作为对象属性的有用验证机制。

例如，根据您的业务需求，您希望内容创建者为属性 cust_news_summary 提供仅 15 个字符长的字母数字值。以下属性中的输入掩码可以在无需任何脚本/编码的情况下实现所需条件：

```
AAAAAAAAAAAAAAA
```

连续的十五个 A 表示只允许 A 类性质的 15 个字符，即字母数字。
请参阅图 11.20，了解 Documentum 提供的所有可用掩码字符和符号：

| Symbol | Interpretation |
|---|---|
| A | 仅允许字母数字字符。即 a-z、A-Z 和 0-9 |
| # | 仅允许纯数字值。即 0-9 |
| ? | 仅允许纯字母值。即 a-z 和 A-Z |
| & | 允许任何 ASCII 字符 |
| U | 允许大写字母字符 |
| L | 允许小写字母字符 |

图 11.20：Documentum 中可用的掩码字符

1. 与我们在对象类型中看到的类似，在约束选项卡中（请参阅图 11.21），您可以使用 Docbasic 表达式为对象类型的属性值指定一致性检查。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_157_0.png)

图 11.21：属性约束选项卡

2. 当您希望用户为属性选择一些预定义值时，值辅助功能非常有用。使用值辅助功能，您可以将属性字段转换为属性屏幕中的下拉列表（选择列表），用户在创建新对象时可以从中选择现有值。单击“添加默认值”按钮（请参阅图 11.22）为属性添加值。默认“值辅助”弹出窗口显示（图 11.23）。

这里值得一提的是，当属性的数据类型为 Boolean 时，值辅助功能不可用。在这种情况下，“添加默认值”按钮为灰色。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_158_0.png)

图 11.22：属性值辅助选项卡

您可以提供各个值，每行一个（按 Enter 键），并通过选中复选框“列表已完成....”将选择列表标记为完整。请参阅图 11.23 了解详情。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_159_0.png)

图 11.23：值辅助固定列表弹出窗口

或者，您可以改选“查询”选项旁边的单选按钮，而不是“固定列表”选项（请参阅图 11.24）。这将允许您使用通过 DQL 查询的对象类型或 Documentum 中的注册表动态填充下拉列表的值。我们将在本章后面详细介绍这一点。现在，请不要关心“查询”选项如何用于查询注册表。对于 cust_news_summary 属性，我们不需要在下拉列表中显示任何预定义值。但是，对于 cust_news_display_order 属性，我们将查询注册表并详细介绍“查询”选项的使用。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_159_1.png)

图 11.24：查询选项的值辅助弹出窗口

## 11.3.2.8 条件值辅助

Documentum 通过允许开发人员根据某些属性值有条件地配置下拉值，进一步改进了值辅助功能。让我们通过一个快速示例来了解 Documentum 中条件值辅助的工作原理。

在为属性提供默认“值辅助”后，“添加条件”按钮将被启用，如图 11.25 所示。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_160_0.png)

图 11.25：使用条件值辅助

单击“添加条件”按钮会显示如图 11.26 所示的值辅助子句弹出窗口。条件可以以 Docbasic 表达式的形式提供（如我们在创建对象类型时在约束选项卡中看到的）。输入的 Docbasic 表达式可以通过单击“检查语法”按钮进行验证。根据输入的 Docbasic 表达式（属性条件）的评估，将在弹出窗口的值辅助部分下为用户提供下拉列表。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_161_0.png)

图 11.26：值辅助子句弹出窗口

您可以通过以与输入第一个条件子句相同的方式提供额外的 Docbasic 表达式来提供额外的条件和子句。所有现有条件均以“如果...则...”子句的形式显示，如图 11.27 所示。

它表示如果 title 属性的值为字母，则下拉列表将显示值：A、B、C。

如果 title 属性的值为数字，则下拉列表将显示值：1、2、3、4、5。

如果没有满足任何条件，则下拉列表中显示的默认值是：1、2、3。

但是，请记住，由于 Docbasic 的限制，您不能在 Application Builder 中提供超过 35 条此类条件值辅助语句。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_162_0.png)

图 11.27：现有条件子句

一个常见的需求是在下拉值中显示一些用户友好的文本，同时为应用程序读取和理解的每个值保存简短代码。

在我们的示例中，我们可能希望用户在下拉列表中看到的值为“One、Two、Three”，而实际上在 Docbase 后台分别将其保存为 1、2 和 3。

在“值映射”选项卡下，如图 11.28 所示，在“数据”列中输入 Docbase 值，并在“显示字符串”列中输入其映射的下拉显示值。

“显示字符串”下的值仅用于在下拉列表中视觉显示。“数据”列中的值将作为属性的值保存在 Docbase 中。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_163_0.png)

图 11.28：属性值映射选项卡

在所有必要的选项卡中填写相关数据后，右键单击对象类型 cust_newsarticle（在左侧树窗格中）并选择选项“检入所选对象(s)”以将对象类型（连同其属性）检入 Docbase。这样做使新创建的属性在 Docbase 中可供应用程序使用。

根据属性设计表 11.16 中的规范创建其他属性 cust_news_is_press_rel 和 cust_news_edit_approvers。我们现在将通过创建属性 cust_news_display_order 如下，介绍使用查询选项的条件值辅助的示例。

> 您可能希望跳回第 3 章，在继续以下部分之前快速回顾我们之前关于注册表的讨论。

## 11.4 使用 DQL 查询注册表以进行值辅助

创建一个新属性 cust_news_display_order 并如 11.29 图所示填写“常规”选项卡。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_164_0.png)

图 11.29：cust_news_display_order 属性常规选项卡

如果属性 cust_news_is_press_rel 的值为 true，则该属性需要显示下拉值：1、3、5、7；如果属性 cust_news_is_press_rel 的值为 false，则显示值：0、2、4、6。

这可以通过条件值辅助和使用注册表来实现，如下所示：

1. 根据以下设计在底层 RDBMS 中创建表：
  - **表名：** cust_values_table
  - **列 1 (int)：** cust_odd_values
  - **列 2 (int)：** cust_even_values

将值插入两列，如图 11.30 所示：

![](img/b18244e6621ecc39cef03ff6c0e55e7c_165_0.png)

图 11.30：cust_values_table 数据库表

2. 以管理员身份登录 Documentum Administrator 并通过以下 DQL 查询注册 RDBMS 表 cust_values_table：

```
register table dm_dbo.cust_values_table (cust_odd_values int,cust_even_values int)
update dm_registered_object set owner_table_permit=15 where object_name='cust_values_table'
update dm_registered_object set group_table_permit=15 where object_name='cust_values_table'
update dm_registered_object set world_table_permit=15 where object_name='cust_values_table'
```

请注意，这里 dm_dbo 是 Docbase 所有者的别名，而权限级别 15 用于选择、插入、更新和删除权限。

现在通过 DQL 查询编辑器在 Documentum Administrator 中发出以下 SELECT 查询，以确定注册表中的值是否返回：

```
select cust_odd_values, cust_even_values from dm_dbo.cust_values_table
```

![](img/b18244e6621ecc39cef03ff6c0e55e7c_165_1.png)

图 11.31：通过 DQL 查询注册表时显示的结果
