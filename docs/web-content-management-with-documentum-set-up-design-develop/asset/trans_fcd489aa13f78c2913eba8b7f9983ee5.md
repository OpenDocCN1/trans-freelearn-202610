
![](img/b18244e6621ecc39cef03ff6c0e55e7c_186_0.png)

图 12.13：为给定生命周期状态提供版本标签值

同样，你可以指定当文档处于 WIP 生命周期状态时需要从文档中移除的以下版本标签：Approved、Active、Expired。这可以通过从 Define Action 向导中选择 Remove Version Label 选项来实现。

此外，你还可以使用相同的 Define Action 向导，将我们之前创建的自定义权限集（Custom WIP ACL）附加到处于 WIP 生命周期状态的文档上。

正如我们在设置 Entry Criteria 选项卡时所看到的，可以指定一个 Docbasic 过程供系统在执行指定的标准操作后执行。

图 12.14 显示了为 WIP 生命周期状态添加的所有标准操作。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_187_0.png)

图 12.14：Lifecycle 状态编辑器 Actions 选项卡下列出的指定操作

## Documentum 5.3 更新

Documentum 5.3 版本允许您在生命周期入口条件、操作、PostChange 以及验证中指定 Java 类作为过程。

7. 在 PostChange 选项卡下（参见图 12.15），您可以指定一个 post-change Docbasic 过程，系统可在状态转换完成后执行该过程。

单击 Docbase Path Name 字段旁显示的省略号按钮，并浏览到正确的 Docbase 位置以指定 Docbasic 过程。

请注意，对于 WIP 生命周期状态，我们已指定执行开箱即用的过程 wcmLifecycleScript，该过程负责多个标准生命周期操作和文档的系统检查。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_188_0.png)

图 12.15：生命周期状态 PostChange 选项卡

8. 指定 PostChange 过程后，您可以转到生命周期状态编辑器中的 Attributes 选项卡（参见图 12.16）。此功能允许您在文档达到特定生命周期状态时修改所选文档属性的某些属性。

选择您希望修改其属性的文档属性。这里我们以选择属性 subject 并单击 Change Properties... 按钮为例。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_189_0.png)

图 12.16：生命周期状态 Attributes 选项卡

属性窗口打开（参见图 12.17），显示所选属性并允许用户以任意组合更改以下内容：

- **Label Text:** 在客户端应用程序（例如 Web Publisher）中显示在属性旁边的文本（描述属性的名称）。
- **Help Text:** 有关该属性的一些帮助文本。
- **Comment:** 属性的简短描述或注释。
- 其他属性，例如设为只读、隐藏等。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_190_0.png)

图 12.17：更改所选属性的属性

9. 生命周期中的文档通过某些客户端应用程序（如 Desktop Client、Web Publisher 等）进行操作。客户端程序将文档的功能（例如：签出、链接到位置等）与实现这些功能的实际组件（功能类）相匹配。

如果您还记得我们关于对象类型的讨论，我们看到对象类型定义包含实现其功能的一定功能类。

使用生命周期状态编辑器，您可以修改与对象类型定义关联的功能类、添加新组件及其实现类以及执行其他此类操作。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_191_0.png)

图 12.18：生命周期状态编辑器 Functionality 选项卡

10. 与设置 WIP 状态类似，请根据图 12.19 所示的定义创建 Start、Staging 和 Approved 生命周期状态。这些基于 Documentum 的默认生命周期，并且可以根据您的具体业务需求进行更改。

创建所有生命周期状态后，您可以像权限集和别名集一样将自定义生命周期签入 Docbase。

Documentum 允许您对文档和生命周期进行版本控制。在某一时刻，多个文档版本可以附加到同一生命周期的多个版本。

例如，假设您有一个新闻文章文档 news_story001.xml，由新闻文章对象类型 cust_newsarticle 创建，并与我们的自定义生命周期 Custom_Lifecycle 相关联。

此时该文档与生命周期版本 1.0 相关联。开发人员修改了生命周期并将修改后的生命周期以版本 1.1 的形式签入 Docbase。

然而，这并不意味着文档 news_story001.xml 现在会自动与生命周期版本 1.1 相关联。它仍然链接到生命周期版本 1.0，这在某些情况下可能会造成混淆。

> 除非在您的业务场景中绝对需要，否则不要以次要版本或主要版本的形式将生命周期签入 Docbase。相反，将生命周期以修改前的同一版本签入，以便文档不会丢失关联。

图 12.19 显示了我们自定义生命周期的生命周期状态定义：

| 生命周期状态 | 通用 | 入口条件 | 操作 | 更改后 | 属性 | 功能 |
|---|---|---|---|---|---|---|
| Start | 允许附加：是<br>允许降级：否<br>允许计划转换：是 | N/A | N/A | N/A | N/A | N/A |
| WIP | 允许附加：是<br>允许降级：是<br>允许计划转换：是 | N/A | 添加版本标签：WIP<br>移除版本标签：Active、Approved、Expired<br>设置权限集：Custom WIP ACL | 过程 Docbase 路径名称：/System/Applications/WebPublisher/wcmLifecycleScript | N/A | N/A |
| Staging | 允许附加：是<br>允许降级：是<br>允许计划转换：是 | N/A | 添加版本标签：Staging<br>移除版本标签：Approved<br>设置权限集：Custom Staging ACL | N/A | N/A | N/A |
| Approved | 允许附加：是<br>允许降级：是<br>允许计划转换：是 |  | 添加版本标签：Approved<br>设置权限集：Custom Approved ACL | 过程 Docbase 路径名称：/System/Applications/WebPublisher/wcmLifecycleScript | N/A | N/A |

图 12.19：各种生命周期状态的定义

11. 将生命周期签入 Docbase 后，请按本章前面讨论的方式安装生命周期，以便应用程序可以使用它。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_193_0.png)

图 12.20：已完成的已安装生命周期

## 12.5 通过生命周期提升和降级内容

附加到生命周期的创建内容可以通过以下一种机制在各种生命周期状态之间推进：

- Web Publisher 的 Promote 和 Demote 菜单选项
- 工作流
- 服务器 API 命令：promote 和 demote
- BATCH_PROMOTE 管理方法

我们将在 Web Publisher 中创建示例内容，并查看如何使用上述机制通过其各种生命周期状态提升和降级它。

使用以下任一方法在 Web Publisher 中创建附加到 Custom_Lifecycle 的新闻文章：

- 使用我们在第 13 章中介绍的新闻文章 Web Publisher 模板
- 通过菜单选项 File | Import 在 Web Publisher Web 文件柜中导入内容文件，并将其附加到自定义对象类型 cust_newsarticle 和生命周期 Custom_Lifecycle（参见图 12.21）

![](img/b18244e6621ecc39cef03ff6c0e55e7c_194_0.png)

图 12.21：导入内容文件并将其附加到 Custom_Lifecycle

您可以通过图 12.22 中显示的 Status 列验证新创建/导入的内容处于 WIP 生命周期状态。选择内容旁边的复选框并选择菜单选项 Document | Lifecycle | Promote。
