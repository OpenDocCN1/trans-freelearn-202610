
## 操作映射

您在大多数情况下会使用操作映射——这是配置物理端口最简单的方式，因为 Consume Adapter Service Wizard 会完成大部分工作。对于设计时您所选择的操作，Consume Adapter Service Wizard 会在 BtsActionMapping 包装器内部生成相应的 Operation 条目，并将该映射放置到它将生成的物理端口绑定文件中。一般形式下，操作映射如下所示：

```
<BtsActionMapping>
  <Operation Name ="Operation1" Action = "Action1" />
  <Operation Name = "Operation2" Action = "Action2"/>
  ...
  <Operation Name = "OperationN" Action = "ActionN"/>
</BtsActionMapping>
```

为了处理操作映射并将 SOAP 操作标签附加到传入消息，WCF-Custom 适配器使用以下算法：

当传出消息离开编排时，编排实例会使用逻辑端口操作名称设置 BTS.Operation 上下文属性。一旦订阅机制从消息框中提取消息并将其交给物理端口进行处理，WCF-Custom 适配器会尝试查找操作映射中 Name 与 BTS.Operation 属性匹配的 Operation。如果找到匹配项，则适配器会将相应的 Action 值附加到消息并将其发送到自定义绑定以执行。如果未找到匹配项，则 WCF-Custom 适配器会将整个 BtsActionMapping 视为一个大的操作，因此自定义绑定将获得以下操作：
`<BtsActionMapping><Operation Name="Operation 1" Action =......</BtsActionMapping>` 其后果相当可预测 – "Invalid action" 异常。因此，规则很明确：对于每个编排传出操作，对应的物理端口操作映射中必须有一个 Operation 条目，其 Name 属性值与编排操作名称匹配。

需要注意的是，对于基于内容路由的场景，其中您不使用编排而仅依赖消息引擎提供的功能，您无需编写自定义管道组件来设置 BTS.Operation 上下文属性。幸运的是，WCF-Custom 适配器支持一种替代的单操作格式。

![](img/23d1402649b6528b085cd6b900a2c9c3_64_0.png)

本质上，这意味着所有到达为单操作处理配置的发送端口的消息都将被标记为相同的操作，而不管 BTS.Operation 上下文属性如何。由于 Echo 适配器仅支持一个操作，因此单操作方法是一种完全适用于我们示例应用程序的替代解决方案。

## 动态设置 SOAP 操作

在您希望将对要执行哪个操作的决策推迟到运行时时，可以使用 WCF.Action 上下文属性。您需要做的就是向编排添加一个 Expression 形状并配置该形状以执行类似于下面所示的代码：

```
YourOutboundMessage (WCF.Action) = "DesiredAction"
```

正如您所见，WCF-Custom 适配器提供了足够的灵活性来应对您可能遇到的几乎任何情况。一旦您理解了概念和可用选项，只需准确操作，不超过几分钟即可配置发送端口。

## ASDK 工具和功能

到目前为止，您基本上已拥有开始构建依赖于基于 ASDK 的适配器的应用程序所需的所有必要信息。然而，与任何严肃的技术一样，ASDK 无疑也是如此，其中一些区域和功能如果被滥用可能会成为头痛的来源或降低您的工作效率。而且，与任何严肃的技术一样，也有工具可以帮助您有效解决潜在问题。

在本节中，我们将讨论以下适用于所有基于 ASDK 的适配器的主题：

- 生成架构 - 有用的提示
- 处理超时
- 跟踪和追踪

### 生成架构

完成演练后，您可能会得出结论，生成架构可能是决定您使用适配器的体验是快速顺畅还是需要花费大量时间排查应用程序问题的最重要步骤。如果这是您的判断，您绝对是正确的。在这里，我们想为您提供一些提示和建议，这可能会提高您的工作效率并帮助避免潜在问题：

- 如果您对 **Consume Adapter Service** 插件生成的架构文件名称不满意，请随意将其更改为您喜欢的名称或贵组织采用的命名约定。
- 正如您可能知道的，BizTalk 架构会被编译成 .Net 类。您可以自由更改要生成的 .NET 类的名称和命名空间。只需在解决方案资源管理器中单击架构文件并在 **Properties** 窗口中进行更改。这样，您将使引用 XSD 文件中定义的类型变得更容易、更直观。
- 您可以根据需要区分或提升架构字段。但是，请注意，如果出于任何原因您需要重新生成架构，您的提升将会丢失。

- 在绝大多数情况下，您无需更改 Consume Adapter Service 插件生成的架构的命名空间和根节点名称。正如您所知，这两个元素构成了文档类型，是消息引擎订阅机制的基石。如果您仍然有非常充分的理由更改类型，则该更改必须伴随自定义管道组件。对于入站场景，管道组件必须将适配器生成的文档转换为匹配您的自定义文档类型。否则，消息引擎将抛出臭名昭著的 "subscription not found" 异常。对于出站场景，您的自定义管道组件必须将您的自定义文档转换为适配器期望的类型，即 Consume Adapter Service 插件为您生成的类型。否则消息将被适配器拒绝。
- 如果您计划在同一 BizTalk 组中使用多个应用程序使用 ASDK 适配器，则必须格外谨慎。如果您在不同的 Visual Studio 项目中为同一操作生成架构，您将最终出现同一消息类型在 BizTalk 管理数据库中出现两次的情况。此配置的后果是消息引擎将抛出异常，报告其无法解析消息类型或找到订阅，因为多个架构匹配了消息类型。
- 为避免这种情况，我们建议将 **Consume Adapter Service add-in** 生成的架构放入单独的 Visual Studio BizTalk 项目中，您可以从其他 Visual Studio BizTalk 项目引用该项目。部署时，您需要将包含架构的程序集添加到单独的 BizTalk 应用程序，例如 `YourOrganization.Common`，然后从组中的其他 BizTalk 应用程序引用该应用程序。
- 另一种选择是配置 XMLReceive 或 XMLTransmit 管道，并在 DocumentSpecNames 属性中设置完全限定的架构名称。

- 使用 **Consume Adapter Service Wizard** 时，您可能注意到 **Consume Adapter Service Wizard** 底部的 **Generate Unique Schema Types** 复选框，并想知道这是否是我们刚刚讨论的问题的更简单解决方案。
- 不幸的是，此复选框处理的是从根本上相似但最终不同的问题。考虑以下场景：您想要为例如名为 **LOBOOperationA** 的操作生成架构。**LOBOOperationA** 的架构使用复杂类型 **CT1** 的参数。假设您关闭了 **the Consume Adapter Service Wizard**，并在稍后重新打开它以生成架构 **for LOBOOperationB**，它使用相同的复杂类型 **CT1**。在您生成 **LOBOOperationB** 的架构并编译项目后，您将收到错误消息，报告同一类型 **CT1** 在不同的 XSD 文件中被定义了两次。为解决这种情况，您可以执行以下操作：
  - 在可能的情况下，将您计划使用的所有操作的架构一次性生成到 Consume Adapter Service addin 中。addin 能够识别这种情况并将共享类型的定义放在单个单独的 XSD 文件中，该文件由主 XSD 文件使用 **Import** 指令引用。
  - 如果您的应用程序正在演进，并且您唯一的选择是在多次运行中生成架构，则选中 **Generate Unique Schema Types** 可以通过为复杂类型生成唯一的命名空间来解决问题。

最佳方法是仅运行一次插件并一次性生成所有架构，但正如经常发生的那样，在实际项目中这种解决方案并不总是可行。因此，在许多情况下，您最好的选择是使用 Generate Unique Schema Types 复选框。即使操作不使用共享类型，保持此复选框选中也不会造成任何损害。
