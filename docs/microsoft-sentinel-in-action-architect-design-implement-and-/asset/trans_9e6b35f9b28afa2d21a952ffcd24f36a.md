

| Name | Description |
|---|---|
| Title | 工作簿的名称。应描述得足够清晰，以便用户能够轻松判断将显示何种信息。 |
| Subscription | 选择此工作簿所在的订阅。它应与您的 Log Analytics 工作区相同。 |
| Resource group | 选择此工作簿所在的资源组。它是否与您的 Log Analytics 工作区位于同一资源组并不重要。例如，您可以为 Microsoft Sentinel 实例的所有资源创建一个资源组，或仅为工作簿创建一个资源组，或两者结合。如果适用，请遵循贵公司关于放置项目的标准。 |
| Location | 选择工作簿所在的区域。您希望将其与 Log Analytics 工作区设为同一区域，以避免产生出站费用。 |

Table 8.1

下面有一个复选框，标签为 Save content to an Azure Storage Account。如果您需要保护工作簿中存储的信息，可以通过将工作簿内容保存到 Azure 存储帐户来保护它。请参阅本章末尾的 Further reading 部分，其中包含有关如何执行此操作的链接。

- 4. 点击 **Save** 保存工作簿。

**Note**

实际上，您在编辑新工作簿之前并不一定需要保存它。通常建议您这样做，以确保您拥有可恢复的已保存副本，以防编辑未按预期工作。

就这样。现在，您需要编辑工作簿，以便可以编辑或删除现有步骤，或添加自己的新步骤。请参阅下一节，我们将在其中介绍可以对工作簿进行修改的更多详细信息。

## Editing a workbook

您有时需要编辑工作簿。正如您在上一节中看到的，您需要编辑从头创建的工作簿以添加所需内容。您也可以编辑从模板创建的工作簿以使其符合您的需求。

如果您尚未查看工作簿，则首先需要查看它。如果您已在查看工作簿，则可以跳过下一步，直接进入编辑部分。

要编辑工作簿，请执行以下步骤：

- 1. 转到 **My workbooks** 或 **Templates** 选项卡。
- 2. 选择相关的工作簿。
- 3. 然后，在工作簿的详细信息窗格中选择 **View saved workbook** 按钮。

> **Note**
> 您不能直接编辑工作簿模板。必须先保存它，然后才能编辑已保存的工作簿。如果您是从头创建的工作簿，则必须转到 **My workbooks** 选项卡，因为这些工作簿不是从模板创建的，仅会显示在那里。

页面顶部有一个包含按钮的标题，如下所示。我们在本节中关心的按钮是左侧第一个，名为 Edit：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_252_0.png)

Figure 8.13 – Saved workbook header bar

下表简要描述了每个按钮：

| Icon | Name | Description |
|---|---|---|
|  | Done Editing | 此按钮将从编辑模式切换到查看模式。所有单独的编辑按钮将消失。 |
|  | Open | 打开另一个仪表板。请参阅 *Managing workbooks* 部分了解更多信息。 |
|  | Save | 保存对该工作簿所做的所有更改。 |
|  | Save as | 使用另一个名称保存此工作簿。 |
|  | Settings | 允许您更改此工作簿的一些高级设置，包括资源、样式、标签和受信任主机。这些主题的讨论超出了本章的范围。 |
|  | Revert changes | 丢弃任何更改并恢复到上次保存的版本。 |
|  | Refresh | 刷新页面。这将重新计算页面上的任何查询。 |
|  | Share | 允许您直接与他人共享此工作簿。这将打开一个窗格，其中包含 URL，您可以复制并发送给他人。请注意，工作簿必须属于 Shared Workbooks 组后才能共享它。如果工作簿未共享，则在共享之前会提示您将其移至 Shared Workbooks。 |
|  | Show pin options | 这将打开固定选项，您可以在其中将整个工作簿固定到仪表板，或将每个可固定的部分固定到仪表板。 |
|  | Advanced editor | 切换屏幕以显示自动生成的 JSON 或 Azure Resource Manager (ARM) 代码。请参阅 *Advanced editing* 部分了解更多信息。 |
|  | Feedback | 这允许您向 Microsoft 提供有关工作簿的反馈。这与在 Azure 门户其余部分的工作方式相同。 |
| ? Help | Help | 在另一个选项卡中打开 Microsoft 工作簿文档页面。 |

Table 8.2

如果您查看正在编辑的工作簿的底部，您将看到与以下屏幕截图相匹配的链接列表。这就是您添加新步骤的方式，每个步骤将在 *Workbook step types* 部分中单独描述：

Add text | Add query | Add metric | Add parameters | Add links/tabs

Figure 8.16 – Edit mode add links

完成所有更改后，点击标题栏中的 **Done Editing** 按钮以恢复到查看模式。所有单独的编辑按钮将消失，已设置为隐藏的任何步骤、参数或列也将消失。

查看您的工作簿，确保刚刚所做的编辑按预期工作。一旦对更改感到满意，请点击 **Save** 按钮保存更改。

## Advanced editing

虽然工作簿的编辑 **graphical user interface (GUI)** 允许您完全创建和编辑 Microsoft Sentinel 工作簿，但有时您可能需要直接在代码中调整设置。您可能还希望获取 ARM 模板，这将允许您在其他位置轻松复制此工作簿或将其作为 DevOps 流程的一部分存储。

在任何一种情况下，点击 **Advanced Editor** 按钮都允许您执行此操作。点击该按钮后，您将被带到高级编辑器的 **Gallery Template** 视图，如以下屏幕截图所示。此视图允许您直接修改 JSON 代码。完成更改后，点击 **Apply** 按钮应用更改，或点击 **Cancel** 按钮返回 GUI 而不保存更改：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_255_0.png)

Figure 8.17 – Advanced Editor – Gallery Template view

下面是所有工作簿的列表。点击任何工作簿将更改您正在查看的工作簿。

每个工作簿将显示在单独的行中。它将显示名称，然后是一个图标，显示工作簿是否已从模板创建，然后是一个上下文相关菜单。点击上下文相关菜单图标将显示如图 8.20 所示的菜单。

此菜单允许您删除此工作簿，重命名它，将其移至 **Shared reports**（或如果此工作簿已共享，则允许您将其移至 **My reports**），与他人共享它（或如果它不是 **shared report**，则在共享之前会要求您使其成为 **shared report**），并将其固定到仪表板，这提供了直接访问此工作簿的快捷方式。所有这些都可以在以下屏幕截图中看到：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_258_0.png)

Figure 8.20 – Managing the workbook context menu

我们已经完成了对如何管理现有工作簿的介绍。您已了解 Saved Workbooks 窗格，以及上下文相关菜单，该菜单允许您对工作簿执行各种管理任务。现在，是时候查看构成工作簿的各个部分以及如何使用它们了。

## Workbook step types

每个工作簿由一个或多个步骤组成。如前所述，工作簿由称为步骤的小节组成。每个步骤都有一个唯一名称，可以是几乎任何内容，并且该名称可以在其他步骤中引用。每个步骤都可以独立运行，尽管有些可能需要参数，无论是从参数步骤还是从其他步骤获取。

共有六种不同类型的步骤：text、query、metric、parameters、links/tabs 和 groups。每种类型的步骤将在以下各节中更详细地讨论。

要在编辑工作簿时添加新步骤，屏幕底部是 +Add 按钮，点击后将显示步骤类型的列表，如以下屏幕截图所示。点击您希望添加的步骤类型的相应选项：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_259_0.png)

Figure 8.21 – Edit mode Add option

无论点击哪个链接，步骤底部都将有一系列选项，如以下屏幕截图所示：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_259_1.png)

Figure 8.22 – Edit mode add options after adding a step

点击 Done Editing 按钮将所选步骤更改为查看模式，以便您可以查看更改的外观。

点击 Add 按钮添加新步骤。

