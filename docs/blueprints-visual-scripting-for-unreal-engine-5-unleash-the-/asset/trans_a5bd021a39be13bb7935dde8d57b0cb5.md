


![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_118_0.png)

图 4.19 – ActorBeginOverlap 事件的操作

### 重要提示

**获取游戏模式** 函数返回当前关卡所使用的 **游戏模式** 的引用。但返回值的类型是 **游戏模式基类**。使用此类型的变量时，我们无法访问 **添加游戏得分** 函数。因此，有必要使用 **转换为 BP_GameModeWithScore**。

- 18. 编译 `BP_Collectable`。在关卡编辑器中，将 `BP_Collectable` 的一些实例拖放到关卡中。单击 **播放** 按钮来测试关卡。使用你的角色收集雕像，并在屏幕上查看当前得分。

### 重要提示

前一张截图中出现的两个白色连接引脚称为 **重路由节点**。可以从 **上下文菜单** 中添加它们，用于帮助组织蓝图。

此实际示例展示了使用 **转换为** 节点的两种常见方法。一种方法是测试实例是否为特定类型。另一种方法是访问子类的变量和函数。现在我们知道如何使用 **转换为**，让我们学习如何在关卡蓝图中添加角色的引用和事件。

## 关卡蓝图通信

虚幻引擎有一种特殊的蓝图，称为 **关卡蓝图**。游戏中的每个关卡都有一个默认的关卡蓝图。它们用于创建仅在当前关卡中发生的事件和动作。要访问关卡蓝图，请单击关卡编辑器工具栏上的 **蓝图** 按钮，并选择 **打开关卡蓝图** 选项，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_119_0.png)

图 4.20 – 打开关卡蓝图

在关卡蓝图中，我们可以轻松创建关卡中角色的引用。为了实际演示，让我们创建一个示例，将 **Box Trigger** 添加到关卡中。当角色与触发器重叠时，**Blueprint_Effect_Sparks** 被激活，产生火花效果：

- 1. 创建或使用基于第三人模板且带有入门内容的现有项目。

- 2. 在关卡编辑器中，单击工具栏上位于 **创建** 按钮。在 **基本** 类别中，有一个 **Box Trigger**，如下图所示。将 **Box Trigger** 拖到关卡中的某个位置并放下：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_120_0.png)

图 4.21 – 创建 Box Trigger

- 3. 调整 **Box Trigger** 的大小，并将其放置在关卡中玩家必须经过的位置。下图显示了一个示例。**Box Trigger** 在游戏中是隐藏的：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_121_0.png)

图 4.22 – 调整大小并放置 Box Trigger

- 4. 确认 **Box Trigger** 已被选中，通过单击关卡编辑器工具栏上的 **Blueprints** 按钮并选择 **Open Level Blueprint** 来打开关卡蓝图。

- 5. 右键单击 **Event Graph** 并添加位于 **Add Event for Trigger Box 1** 类别中、标记为 **Add OnActorBeginOverlap** 的事件，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_122_0.png)

图 4.23 – 为触发框添加事件

- 6. 返回关卡编辑器。在内容浏览器中，转到 Content | StarterContent | Blueprints 并打开 Blueprint_Effect_Sparks。

- 7. On the Components tab, select the Sparks Component and in the Details tab, search for the auto activate attribute and uncheck it, as shown in the following screenshot. We did this so that the Sparks starts inactive so we can activate it at runtime. Compile Blueprint_Effect_Sparks：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_123_0.png)

图 4.24 – 禁用火花组件的自动激活

- 8. 在关卡编辑器中，将 **Blueprint_Effect_Sparks** 从 **Content Browser** 拖放到关卡中靠近 **Box Trigger** 的位置以创建实例。

- 9. Confirm that **Blueprint_Effect_Sparks** is selected and open the Level Blueprint. Right-click on **Event Graph** and select **Create a Reference to Blueprint_Effect_Sparks**，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_124_0.png)

图 4.25 – 创建蓝图效果火花的引用

- 10. Drag from the **Blueprint_Effect_Sparks** blue pin of the node and drop it in the graph to open **Context Menu**。搜索 `activate` 并选择 **Activate (Sparks)**。Connect the white pin of the **OnActorBeginOverlap (TriggerBox)** event to the white pin of the **Activate** function，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_124_1.png)

图 4.26 – 重叠触发框时激活火花

- 11. Compile the Level Blueprint and click the **Play** button of the Level Editor to test the Level. Move your character to the location of the **Box Trigger** to activate the sparks.

在本示例中，我们了解了如何在关卡蓝图中添加角色的引用和事件。这就是 **关卡蓝图通信** 的精髓。

蓝图和关卡蓝图之间还存在另一种通信形式，称为事件分派器。

## 事件分派器

**事件分派器** 允许一个 **蓝图** 在 **事件** 发生时通知其他蓝图。**关卡蓝图** 和其他蓝图类可以监听此 **事件**，并且在 **事件** 触发时可能有不同的 **动作** 运行。

我们在 **My Blueprint** 面板中创建事件分派器。作为示例，让我们创建一个名为 BP_Platform 的蓝图。当角色与 BP_Platform 蓝图重叠时，它会调用一个名为 PlatformPressed 的事件分派器。关卡蓝图正在监听 PlatformPressed 事件，并在触发该事件时生成一个爆炸：

- 1. 创建或使用基于第三人模板且带有入门内容的现有项目。
- 2. 创建一个蓝图并使用角色作为父类。将其命名为 BP_Platform 并在蓝图编辑器中打开它。
- 3. Click the Add button in the Components panel and choose the Static Mesh Component. In the Details panel, choose the Shape_Cylinder Static Mesh and change the Z value of the Scale attribute to 0.1. Also, change Collision Presets to OverlapAllDynamic, as shown in the following screenshot：

| 属性 | 值 |
|----------|-------|
| 缩放 Z | 0.1 |
| 静态网格 | Shape_Cylinder |
| 碰撞预设 | OverlapAllDynamic |

图 4.27 – 设置静态网格

- 4. Compile the Blueprint. In the **My Blueprint** panel, create an **Event Dispatcher** and name it `PlatformPressed`。An Event Dispatcher can have input parameters. Let's create one to send a reference of the `BP_Platform` instance that was overlapped. In the **Details** panel, create a new parameter in the **Inputs** category, name it `BP_Platform`，并将其设置为 `BP Platform` 类型的对象引用，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_127_0.png)

图 4.28 – 创建输入参数

- 5. Right-click **Event Graph** and add **Event ActorBeginOverlap**。Drag the `PlatformPressed` Event Dispatcher and drop it in **Event Graph**。Choose **Call** in the submenu。Right-click **Event Graph**，search for `self`，and select the **Get a reference to self** action。The **Self Action** returns a reference of the current instance。Connect the **Actions**，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_127_1.png)

图 4.29 – 调用 Platform Pressed 事件分派器

- 6. Compile the Blueprint。In the Level Editor，drag BP_Platform from Content Browser and drop it in the Level to create an instance。

- 7. Make sure the BP_Platform instance is selected and click the **Blueprint** button at the toolbar of the Level Editor and choose Open Level Blueprint。Right-click Event Graph and select Add Platform Pressed，如下图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_128_0.png)

图 4.30 – 在关卡蓝图中添加 Platform Pressed

- 8. Right-click Event Graph，search for spawn actor，and select Spawn Actor from Class。Click the drop-down menu in the Class parameter and select Blueprint Effect Explosion。Drag from the blue pin of the PlatformPressed (BP_Platform) Event，drop it in the graph to open Context Menu，and choose the GetActorTransform Action。Connect the nodes，如下图所示：

