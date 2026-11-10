
## 11 设计和创建自定义对象类型

我们之前讨论过，Documentum 中的几乎所有内容都被视为对象，而对象不过是对象类型的实例。Documentum 成功实施的关键在于良好地设计自定义对象类型。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_140_0.png)

图 11.1：Documentum 对象类型的逻辑表示

如第 2 章所述，Documentum 对象类型遵循层次协议。子类型从超类型扩展而来，并且本身也可以成为另一个对象类型的超类型。图 11.2 显示了 Documentum 中对象类型层次结构的快照。注意，虽然 SysObject（dm_sysobject）在 Documentum 中是 Persistent 对象的子类型，但它是诸如文件夹、文档、作业和方法等对象的超类型。尽管图中未显示，文件夹对象（dm_folder）本身又是柜子对象（dm_cabinet）的超类型。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_141_0.png)

图 11.2：Documentum 中的示例对象类型层次结构

在了解了 Documentum 对象的基础知识后，让我们继续以虚拟新闻网站的案例研究为例，为中心窗格新闻文章设计/创建一种对象类型。

### 11.1 创建对象类型

在本节中，我们将详细介绍创建对象类型的步骤。首先需要确定应用程序所需的对象类型。对于中心窗格文章，我们需要一个对象类型，例如 cust_newsarticle（custom news article 的缩写），并且新闻文章模板将与此对象类型关联。要创建新的对象类型，请在 DocApp 构建器中选择 Insert | Object Type，并在 Type name 下指定名称 cust_newsarticle，如图 11.3 所示。新的对象类型以签出模式显示在 Application Builder 左侧树窗格中的 Object Types 文件夹下。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_142_0.png)

图 11.3：创建新的对象类型

### 11.1.1 类型名称

对象类型名称在 Docbase 中是唯一的且全局可访问的，因此为自定义对象类型采用智能命名约定至关重要，以便将其与 Docbase 中数千个现有对象区分开来。此外，在将对象类型部署到其他 Docbase 时，良好的命名约定可避免在目标 Docbase 中出现潜在的命名冲突。

### 11.1.2 对象类型名称的限制

图 11.4 显示了我们在命名对象类型时需要牢记的一些限制。

| 限制 |
|---|
| 不应以 'dm_' 开头，因为 Documentum 使用命名方案 dm_ 为其内部对象命名 |
| 不应以以下开头：空白字符、单引号 (') 或数字 |
| 长度最大为 18 个字符 |
| 不应使用与 Documentum DQL 中的保留字相同名称（例如：CABINET、DELETED、SELECT、INTEGER 等。） |

图 11.4：对象类型命名规范限制

对象类型“常规”选项卡如图 11.5 所示：

![](img/b18244e6621ecc39cef03ff6c0e55e7c_143_0.png)

图 11.5：对象类型“常规”选项卡

### 11.1.3 超类型

每个对象类型可以从 Documentum 中现有的对象类型扩展，也可以将其超类型设为 NULL。

在我们的案例中，新闻文章是一个文档对象类型，因此我们将其从 dm_document 对象类型扩展而来。Super Type 下拉列表中显示的其他对象类型有：dm_job、dm_folder、dm_user 等。

Docbase 中的每个对象类型都存储在其底层 RDBMS 的独立数据库表中。

作为经验法则，不要将对象类型层次结构扩展得太深（超过三层），因为在内部这意味着 Documentum 需要执行相应数量的数据库连接。例如，您可以让自定义对象类型 cust_newsarticle 从 dm_document 扩展，并进一步让另一个自定义对象类型（例如 cust_newsarticle_sub）从 cust_newsarticle 扩展，但不能更深！

> 建议在自定义对象类型上仔细决定名称和超类型，因为在 Docbase 中签入对象类型后，您无法更改其名称或超类型。

现在按照以下步骤完成对象类型的创建：

- 1. 在 Display Configuration 选项卡下，可以配置在特定条件下为特定对象类型显示的特定属性。
- 但是请注意，此功能仅适用于 WDK 5.2 + 应用程序（如 Webtop、Web Publisher 等）和 Desktop 5.2 + 应用程序（如 Desktop Client）。
- 您可以在图 11.6 中看到，默认情况下为 DcDesktopClient（桌面）、Web Publisher 和 Webtop（WDK 客户端）显示了三个应用程序范围。
- 对于这些范围中的每一个，您可以选择需要在属性屏幕中各自选项卡下显示的特定属性（显示在图 11.6 的底部部分）。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_144_0.png)

图 11.6：对象类型显示配置选项卡

Web Publisher 中的属性屏幕（参见图 11.7）显示了各种选项卡，如 Info、Publishing、History 等。这些选项卡下显示的属性可以通过对象类型中的显示配置部分进行配置。

## 设计和创建自定义对象类型

![](img/b18244e6621ecc39cef03ff6c0e55e7c_145_0.png)

图 11.7：Web Publisher 中的属性屏幕选项卡

在 Display Configuration List 下，您可以通过分别单击 Add 或 Rename 按钮来引入新选项卡或重命名现有选项卡，如图 11.6 所示。您甚至可以通过 Remove 按钮完全删除一个选项卡。

在 Attributes in... 显示配置部分中，您可以通过单击 Edit 按钮来选择要为所选选项卡显示的特定属性。

- 2. 在 Security 选项卡内（参见图 11.8），有一个选项可以将默认权限集（访问控制列表）与对象类型关联。这确保了每当从此对象类型创建对象时，它将被分配我们为该对象类型设定的默认权限集。

这是控制特定对象类型对象的访问或修改权限级别的良好机制

要指定权限集，请从 Name: 下拉列表中选择现有权限集。所有其他字段，如 Owner、Description、Permissions 和 Your Permission 均为只读字段，在您从下拉列表中选择权限集后由 Documentum 自动填充。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_146_0.png)

图 11.8：对象类型“安全”选项卡

- 3. 在 Constraints 选项卡内（参见图 11.9），您可以使用 Docbasic（Documentum 的专有语言）表达式为对象类型的属性值指定一致性检查。在 Constraint expression 部分输入 Docbasic 表达式。

假设我们需要确保每当从对象类型 cust_newsarticle 创建内容对象时，其主题属性绝不能留空。在这种情况下的 Docbasic 表达式将为：

```
not(subject = "")
```

输入 Docbasic 表达式后，单击 Check Syntax 以确认输入的 Docbasic 表达式具有正确的语法。您可以参考 Documentum Docbasic 参考手册以了解 Docbasic 及其应用用法。

如果未满足属性约束，则应用程序需要向用户显示的错误消息应输入在 Error message string... 部分。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_147_0.png)

图 11.9：对象类型“约束”选项卡

在我们的案例中，如果内容对象的 subject 属性留空，则会向内容创建者显示错误消息 Please provide a subject。

您可以通过单击 Add Constraint 按钮添加多个约束。执行此操作后，后退和下一步按钮将被启用，允许我们在添加的约束之间前后移动。

- 4. 对象类型继承了其所扩展的超类型的大部分功能。此外，默认 DocApp 的组件用于实现从超类型继承的大部分功能。

使用 Documentum Application Builder，您可以执行各种操作，例如：

- 添加新的功能类及其用于实现的组件。
- 指定不同的组件来实现从超类型继承的功能类。
- 完全禁用继承的功能类。

