

右键单击 FEM 树视图中的 **Document Class** 节点并选择 **New Class**。**Create a Class Wizard** 将启动。

![](img/2295191c450a0c86b2d82df6cec04526_138_0.png)

第一个面板要求输入 **Name**（实际上是指显示名称）和 **Symbolic Name**。当您输入显示名称时，FEM 会自动构建一个默认的符号名称，通过删除空格和其他不可接受的字符。您可以将符号名称修改为其他名称；我们添加了前缀 **Wjc**，因为符号名称必须唯一，使用简短的前缀可以减少与其它名称冲突的可能性。描述是可选的，但您可以提供您想要的任何描述性文本。

下一个面板显示类上的继承属性，并邀请您添加更多属性。由于我们之前尚未创建属性模板，请单击 **New** 按钮以启动 **Create a Property Template** 向导。

## Administrative Tools and Tasks

选择列表没有符号名称，但显示名称要求唯一。我们将选择列表称为 **Literature Formats**。这里对引用完整性真正重要的其实是选择列表的 Id，但 FEM 会自动创建必要的链接，并且不会让我们操心。单击 **Next** 按钮并选择 **String** 作为选择列表数据类型。再次单击 Next 以进入 **Add Choice List Elements** 面板。

![](img/2295191c450a0c86b2d82df6cec04526_140_0.png)

如果我们要制作一个分层选择列表，我们会使用 **New Groups** 按钮。我们只制作一个简单的列表，因此请单击 **New Items** 按钮。

![](img/2295191c450a0c86b2d82df6cec04526_141_0.png)

使用 **Add Item** 面板添加所示的三个值。使用选择列表的应用程序通常会向用户显示 **Name**，但 **Value** 实际上将被用作属性值。当您完成 **Create a Choice List** 向导后，您将被返回到已在进行的 **Create a Property** 向导。

![](img/2295191c450a0c86b2d82df6cec04526_141_1.png)

选择 **Single**（这是属性基数）并单击 **More** 按钮以定义 WjcFormat 属性的额外特征。

![](img/2295191c450a0c86b2d82df6cec04526_142_0.png)

在此面板中最重要的事项是，我们已选中 **Value Required** 复选框，我们已将属性设为 **Read/Write**，并且我们指定了 **Maximum String Length** 为 10 个字符。

当您完成 **Create a Property** 向导后，您将被返回到已在进行的 **Create a Class Wizard**，位于 **Select Properties** 面板。在继续之前，让我们再创建三个新属性，这些属性将在 *Chapter 6* 的一些示例中出现。

- **Genre**（WjcGenre），一个多值字符串属性（唯一且无序）。它应具有 **Value Required**，为 **Read/Write**，并且 **Maximum String Length** 为 25。使用名为 Literature Genre 的选择列表，其值为 Drama、Comedy、Farce、Science Fiction、Western、Romance、Sonnet、Epic、Unknown 和 History。
- **AuthorLastName**（WjcAuthorLastName），一个简单的字符串属性。它应具有 **Value Required**，为 **Read/Write**，并且 **Maximum String Length** 为 64。它没有选择列表。
- **AuthorFullName**（WjcAuthorFullName），一个简单的字符串属性。它不应具有 **Value Required**，为 **Read/Write**，并且 **Maximum String Length** 为 64。它没有选择列表。

您的 **Create a Class Wizard** 应如下所示：

![](img/2295191c450a0c86b2d82df6cec04526_143_0.png)

最后，**Create a Class Wizard** 的最后一个面板将显示您在过程中定义的事物的摘要。单击 **Finish** 按钮以创建类。

![](img/2295191c450a0c86b2d82df6cec04526_143_1.png)

我们不会详细介绍此对话框，因为它更多地涉及 FEM 用户界面而不是 CM。值得注意的是，您可以使用类似 SQL 的查询语言对 CE 服务器进行查询。事实上，我们称之为 CE SQL；它是一个 SQL-92 变体。使用这些查询在应用程序中非常常见，您可以手动从 FEM 中的此对话框执行此操作。在幕后，FEM 正在组合一个 CE SQL 查询。

该对话框的另一种模式允许您简单地粘贴自己构建的查询。从 Content Engine Query Builder 中选择 View > SQL View。

![](img/2295191c450a0c86b2d82df6cec04526_145_0.png)

 resulting view 预填充了一个相当复杂的 CE SQL 查询字符串。您可以删除它并输入或粘贴您自己的查询字符串。

![](img/2295191c450a0c86b2d82df6cec04526_145_1.png)

当您单击 OK 时，查询将被执行，FEM 将显示结果。还有一个选项可以保存查询的本地副本，以便以后加载和重新执行。（此功能与 Workplace 和 XT 功能称为 Stored Search 无关。）

> FEM 查询界面的一个历史怪癖是 SELECT 列表必须以 This 属性开头。这不是 CE SQL 的一般要求。

## Summary

在本章中，我们简要浏览了 CM 的一些常见管理功能。大多数人通过 FEM 工具体验这些功能。然而，FEM 所做的一切都是通过 CE API 完成的，因此也可以编写自定义应用程序来执行这些事情中的一些或全部。

CM 的内容远不止这些，但在我们查看它们之前，我们必须先安装几个更多组件。我们将在第 5 章《Installing Other Components》中继续我们的 CM 安装冒险。

| Component | Linux | Other Unix | Windows |
|---|---|---|---|
| CE server | **Yes** | Yes | Yes |
| CE client | **Yes** | Yes | **Yes** |
| FEM | No | No | **Yes** |
| CSE | **Yes** | Yes | Yes |
| SD | **Yes** | Yes | Yes |
| PE server | No | Yes | **Yes** |
| PE client | **Yes** | Yes | Yes |
| AE | **Yes** | Yes | Yes |
| XT | **Yes** | Yes | Yes |
| RE | No | No | **Yes** |

目前，不要太担心表格第一列中的缩写，尽管它们与我们在 *Chapter 3* 的概述图中使用的相同。我们将在各自的部分中逐一解释它们。

> 我们现在将把我们的全包安装分散到两台不同的机器上。（我们希望你在思考这句话时背对着我们。）为这些机器设置智能的方式是使用静态 IP 地址，如果你能轻松做到，那是正确的方法。如果你做的是我们经常做的事情——使用动态分配的 IP 地址为虚拟机镜像——那么你可能希望在本地 hosts 文件中添加条目。
> 
> 在 Linux 上，它是 `/etc/hosts`，在 Windows 上它是 `C:\WINDOWS\system32\drivers\etc\hosts`。（我们很难记住 Windows 路径，所以我们在 Windows 桌面上放了一个快捷方式。）在任何一种情况下，格式都相同，条目看起来像这样：
> 
> ```text
> 10.0.129.12 wjc-w2k3 wjc-w2k3.example.net
> 10.0.129.29 wjc-rhel wjc-rhel.example.net
> ```
> 
> 如果你的 DHCP 服务器突然为任一主机分配了新地址，你只需更新这两个文件，你就可以通过名称相互引用它们而恢复正常。

![](img/2295191c450a0c86b2d82df6cec04526_148_0.png)

## Content Search Engine (CSE)

CSE 是一个全文搜索引擎。您可以有选择地为文档正文内容以及选定的字符串属性启用全文索引。在 CM 术语中，您有时会看到文本索引的概念被称为 **content-based retrieval (CBR)**。这只是同一事物的另一个名称。CSE 的内部是 Verity 的 K2 引擎，但 Verity 几年前被 Autonomy 收购，所以您会在文档中看到它以三种方式提及——Verity、K2 和 Autonomy。

CSE 不是一个 J2EE 应用程序，并且与 CE 分开运行。在生产环境中，它在与 CE 不同的服务器上运行，以便更好地管理负载。在我们轻负载的全包环境中，我们将在安装 CE 的同一台 Linux 服务器上安装它。当您查看 PPG 中的 CSE 主题时，您会发现构成 CSE 环境的各种不同逻辑服务器类型。对于生产环境，您肯定希望仔细考虑这些信息，但我们的单台 Linux 系统将在我们的安装中容纳所有这些服务器角色。

> 有一个通用要求：如果 CE 在 Windows 上，则 CSE 也必须在 Windows 上。如果 CE 在 Linux 或 Unix 上，则 CSE 必须在 Linux 或 Unix 上。重要的区别是 Windows 与非 Windows；例如，您可以在 Linux 上运行 CE，并在 Unix 上运行 CSE。

![](img/2295191c450a0c86b2d82df6cec04526_149_0.png)

实现一个可工作的 CSE 涉及以下步骤：

- 1. 运行 CSE 安装程序
- 2. 从命令行手动配置一些内容
- 3. 通过 K2 Dashboard 配置 CSE
- 4. 配置 CE（通过 FEM）以了解 CSE 配置

第一次操作时，这些步骤可能看起来有点繁琐甚至神秘，但您可以 mostly 盲目地遵循记录的步骤，事情就会按预期工作。对我们来说，毫无疑问，CSE 安装是正常 CM 设置中最复杂的部分，但我们敦促您不要跳过它。基于文本的搜索在近年来已变得越来越受欢迎。

