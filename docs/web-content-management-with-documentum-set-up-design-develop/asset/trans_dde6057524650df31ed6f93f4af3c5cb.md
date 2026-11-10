
## 创建生命周期、别名集和权限集

![](img/b18244e6621ecc39cef03ff6c0e55e7c_195_0.png)

图 12.22：推广内容

系统需要花费一点时间将内容提升到生命周期中的下一个正常状态。您可以点击“任务状态”按钮查看生命周期提升操作的进度。系统提升内容后，内容将显示在其下一个正常生命周期状态（以 Custom_Lifecycle 为例为 Staging），如图 12.23 所示。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_195_1.png)

图 12.23：降级内容

为了将内容从其当前正常生命周期状态降级，请选中内容旁边的复选框并选择菜单选项 Document | Lifecycle | Demote（参见图 12.23）。

为了强制提升内容，请选中内容旁边的复选框并选择菜单选项 Document | Lifecycle | Power promote。强制提升会将内容直接推送到其 Approved 生命周期状态（即生命周期中的最后状态）。此时，讨论 Web Publisher Monitor_Lifecycles 作业至关重要（参见图 12.24）。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_196_0.png)

图 12.24：Documentum 中的 Monitor_Lifecycles 作业

Documentum 中的 Monitor_Lifecycles 作业按预定义间隔运行并执行以下功能：

如果 Approved 对象的生效日期（a_effective_date 属性）与当前系统日期匹配，Monitor_Lifecycles 作业会通过将其版本标签更改为 Active 使对象处于活动状态。

如果 Active 对象的到期日期（a_expiration_date 属性）与当前系统日期匹配，Monitor_Lifecycles 作业会通过将其版本标签更改为 Expired 使对象过期。

默认情况下，如果内容的生效日期为空，强制提升会立即使文件处于活动状态并发布内容。但是，通过提供 Web Publisher 系统设置，您可以更改此行为。

以管理员身份登录 Web Publisher，并在 Lifecycles 部分下进入 Administration | Web Publisher Admin | Settings。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_197_0.png)

如图 12.25 所示，如果您选中 Delay Publish Active 复选框，则提升到 Active 状态的强制提升将由 Monitor_Lifecycles 作业处理。

以下 API 命令（通过 Documentum Administrator 中的 IAPI 实用程序或 API Tester 执行）也可用于推广和降级内容。请参阅第 26 章以更好地了解服务器 API 命令。

- **推广内容**
  `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote.xml`
  `090015558000bc5b`
  请注意，API 命令 id 返回与指定限定条件匹配的对象的对象 ID。在我们的例子中，090015558000bc5b 是名称为 Testing_Promote_Demote.xml 的新闻文章内容的对象 ID（r_object_id 属性）。
  `API>promote,c,090015558000bc5b`
  `...`
  `OK`
- **降级内容**
  `API>demote,c,090015558000bc5b`
  `...`
  `OK`
- **批量推广内容**
  管理方法 BATCH_PROMOTE 可通过简单的 API 命令将多个对象提升到其下一个生命周期状态。BATCH_PROMOTE 的有趣之处在于它可用于存在于不同生命周期状态并附加到不同生命周期的多个对象。您可以在一次 BATCH_PROMOTE 执行中提升多达 200 个对象。
  现在让我们使用 BATCH_PROMOTE 提升两个内容文件 Testing_Promote_Demote.xml 和 Testing_Promote_Demote002.xml：
  `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote002.xml`
  `...`
  `090015558000bc71`
  `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote.xml`
  `...`
  `090015558000bc5b`
  获取两个内容文件的 r_object_id 后，发出以下简单的 API 命令进行批量提升。
  `API> apply,c,NULL,BATCH_PROMOTE,ARGUMENTS,S,090015558000bc71,090015558000bc5b`

BATCH_PROMOTE 方法反过来调用 dm_bp_batch Documentum 方法，并在执行后返回包含一个结果对象的集合。

### 12.5 过期内容

有时，网站上出现的内容在内容过时或需要进行任何更改时需要从网站上移除。Documentum 提供了一些机制，允许您使内容过期并将其从网站上移除。

首先，您需要了解 Documentum 提供了 SysObject 属性 a_expiration_date，内容作者可以填写该属性以指定内容需要过期的时间。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_198_0.png)

图 12.26：创建内容期间显示的到期日期字段

图 12.26 显示了在 Web Publisher 中创建新内容时，新内容屏幕中的到期日期字段。如果您在创建内容时忘记指定到期日期，您始终可以通过属性（信息）屏幕中的 Publishing 选项卡指定它。

以下是过期 Documentum 中创建内容的某些机制：

- 指定了到期日期的内容的 Monitor_Lifecycles 作业
- Web Publisher Expire now 菜单选项

当活动内容的到期日期与当前系统日期匹配时，Monitor_Lifecycles 作业会使内容过期，并在发布后将其从网站上移除。

使用 Web Publisher Expire now 菜单选项（Document | Lifecycle | Expire now），您可以在其指定的到期日期之前使内容过期，如图 12.27 所示。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_199_0.png)

图 12.27：在 Web Publisher 中手动过期内容项

请注意，如图 12.28 所示，内容的版本标签更改为 Expired（参见状态列）。过期的内容仍保留在 Docbase 中，尽管它已从活动网站中移除。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_199_1.png)

图 12.28：在 Web Publisher 中看到的过期内容

> 为了对处于 Active 状态的内容进行更改，请先使用 Expire now 功能使内容过期，然后再根据需要进行编辑。值得注意的是，以下状态的内容无法降级：WIP、Approved、Active 和 Expired。

这完成了我们对生命周期及其在 Documentum 中用法的讨论。您可以使用生命周期与工作流结合使用。虽然生命周期只是表示文档生命中的阶段，例如草稿、审阅和已批准，但工作流实际上将文档路由通过这些生命周期阶段，并定义谁在何时以及在什么条件下处理文档。从本质上讲，生命周期和工作流互为补充。我们将在第 19 章讨论工作流。现在，您已准备好开始创建 Web Publisher 模板并将其与自定义对象类型关联。从这些创建的内容对象可以与本章中创建的自定义生命周期关联。

### 12.6 摘要

在本章中，我们详细讨论了 Documentum 别名集、权限集（ACL）和生命周期。我们提到了为生命周期创建别名集、为生命周期的各种状态创建权限集以及自定义生命周期对象的详细步骤。

我们还看到了 Documentum 如何在生命周期范围、工作流范围、会话范围、用户范围和系统范围内解析别名的别名值。

本章中创建的自定义生命周期包含四个状态：Start、WIP、Staging 和 Approved，并将 dm_document 作为链接到的主要对象类型。我们将 News Article 对象类型添加为生命周期中的可接受子类型，允许从 News Article 对象类型创建的对象附加到此自定义生命周期。

最后，我们看到了在生命周期的不同状态下提升、强制提升和降级对象的各种机制，以及从网站过期内容的某些方法。
