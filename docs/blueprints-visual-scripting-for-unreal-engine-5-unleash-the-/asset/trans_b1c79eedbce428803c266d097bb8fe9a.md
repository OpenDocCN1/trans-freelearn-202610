

- 14. 从 Event 参数拖出一根线并添加自定义事件。将其命名为 TryToSpawnEnemy。
- 15. 从 TryToSpawnEnemy 的白色引脚拖出一根线并添加 Get All Actors Of Class 节点。在 Actor Class 参数中，选择 BP_EnemyCharacter。
- 16. 从 Get All Actors Of Class 的白色输出引脚拖出一根线并添加 Branch 节点。
- 17. 从 Branch 节点的 True 引脚拖出一根线并添加 Spawn Enemy 宏节点。
- 18. 从 Get All Actors Of Class 的 Out Actors 引脚拖出一根线并添加 Length 节点。Length 节点的返回值将是关卡中敌人的数量。
- 19. 从 Length 节点的输出引脚拖出一根线并添加 Less 节点。将 Less 节点的输出引脚连接到 Branch 节点的 Condition 参数。
- 20. 从 Less 节点的底部引脚拖出一根线并添加 Get Max Enemies 节点。
- 21. 编译、保存并关闭蓝图编辑器。从内容浏览器中拖动 BP_EnemySpawner 并将其放置在关卡中的任意位置以创建实例。单击 Play 按钮测试敌人生成。

运行游戏时，您会定期看到新敌人出现。不过请注意，生成后的敌人不会移动，除非它们听到或看到玩家。这是因为它们在创建时没有设定要追踪的巡逻点。我们不是为生成的敌人添加巡逻点，而是要为敌人的导航行为增加随机性。

## 创建敌人漫游行为

在 `Chapter 9`，`Building Smart Enemies with Artificial Intelligence` 中，我们将敌人的默认行为设定为在两点之间进行巡逻移动。虽然这作为听觉和视觉组件的测试平台效果不错，并且适合潜行类游戏，但我们将通过用随机漫游取代此行为来提升游戏体验的挑战性和动作感。这将使躲避敌人变得困难得多，并鼓励更直接的对抗。为此，我们将返回到 `BT_EnemyBehavior` 行为树。

## 使用自定义任务识别漫游点

我们需要在 `BB_EnemyBlackboard` 中创建一个键，用于存储敌人下一个漫游目的地的位置。与 `PatrolPoint` 键不同，我们的目的地不会由游戏内的角色表示，而是由向量坐标表示。然后，我们将创建一个任务来确定敌人应该在关卡的哪个位置漫游。

创建键和任务的步骤如下：

- 1. 在内容浏览器中，双击 `BT_EnemyBehavior` 资产以打开行为树编辑器。
- 2. 单击 **Blackboard** 选项卡以编辑 `BB_EnemyBlackboard`。
- 3. 单击 **New Key** 按钮，并选择 **Vector** 作为键类型。将此新键命名为 `WanderPoint`:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_381_0.png)

图 10.28 – 在黑板中创建 WanderPoint 键

- 4. 保存黑板并单击 **Behavior Tree** 选项卡以返回行为树。
- 5. 单击工具栏上的 **New Task** 按钮，并在出现的下拉菜单中选择 **BTTask_BlueprintBase** 选项。
- 6. 在内容浏览器中，将新创建的 **BTTask_BlueprintBase_New** 资产重命名为 **BTTask_FindWanderPoint**。双击 **BTTask_FindWanderPoint** 以返回蓝图编辑器。
- 7. **Details** 面板显示类默认值。将 **Node Name** 字段更改为 `FindWanderPoint`。
- 8. 在 **My Blueprint** 面板中，单击 **Variables** 类别的 + 按钮。在 **Details** 面板中，将变量命名为 `WanderKey`，将其类型更改为 **Blackboard KeySelector**，并选中 **Instance Editable** 属性:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_382_0.png)

图 10.29 – 创建 WanderKey 变量

- 9. 将鼠标悬停在 **My Blueprint** 面板的 **Functions** 类别上，单击 **Override** 下拉菜单，并选择 **Receive Execute** 以添加事件。
- 10. 在 **Event Receive Execute** 中，我们将获取关卡的随机位置并将其存储在黑板中:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_382_1.png)

图 10.30 – Event Receive Execute 动作

- 11. 从 **Event Receive Execute** 的白色引脚拖出一根线并添加 **Set Blackboard Value as Vector** 节点。
- 12. 从 **Set Blackboard Value as Vector** 的白色输出引脚拖出一根线并添加 **Finish Execute** 节点。选中 Finish Execute 节点的 **Success** 参数。
- 13. 从 Set Blackboard Value as Vector 的 Key 参数拖出一根线并添加 Get Wander Key 节点。
- 14. 从 Set Blackboard Value as Vector 的 Value 参数拖出一根线并添加 GetRandomPointInNavigableRadius 节点。将 Radius 设置为 10000.0。
- 15. 从 Origin 参数拖出一根线并添加 Make Vector 节点。
- 16. 编译并保存蓝图。

现在我们有了自定义任务，可以修改 `BT_EnemyBehavior` 来让敌人找到并移动到 `WanderPoint`。

## 将漫游添加到行为树中

我们将把 Move To Patrol 序列转换为 Wander 序列。行为树中的新 Wander 序列由以下节点表示:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_383_0.png)

图 10.31 – 新的 Wander 序列

修改行为树的步骤如下：

- 1. 在内容浏览器中，双击 BT_EnemyBehavior 资产以打开行为树编辑器。
- 2. 选择 **Move To Patrol** 序列节点。在 **Details** 面板中，将 **Node Name** 更改为 `Wander`。
- 3. 选择 Move To 任务节点，并将 **Blackboard Key** 更改为 `WanderPoint`。
- 4. 从 **Wander** 序列节点向其他节点左侧拖出一根线并添加 BTTask_FindWanderPoint 节点。
- 5. 在 **Details** 面板中，将 **Wander Key** 设置为 `WanderPoint`。同时，将 **Node Name** 更改为 `Get Next Wander Point`。
- 6. 保存并关闭行为树编辑器。

这是我们在行为树中需要更改的所有内容，以包含敌人漫游行为。我们需要在两个蓝图中进行一些调整。

## 最后调整和测试

我们将删除 BP_EnemyCharacter 中用于设置 `Patrol Point` 的 **Event BeginPlay**。我们不再需要它，因为我们使用的是随机 `WanderPoint`。

我们需要做的另一项更改是修改 `FirstPersonCharacter` 的 **Target Goal** 值，使其更高，以便游戏可以持续更长时间。我们将此值设置为 `20`，因此玩家必须消灭 20 个敌人才能获胜。

调整蓝图的步骤如下：

- 1. 打开 BP_EnemyCharacter 蓝图。
- 2. 在事件图中，删除 **Event BeginPlay** 及其连接的所有节点。
- 3. 编译、保存并关闭蓝图编辑器。
- 4. 打开 `FirstPersonCharacter` 蓝图。
- 5. 在 **My Blueprint** 面板中，选择 **TargetGoal** 变量。在 **Details** 面板中，将 **DEFAULT VALUE** 更改为 `20`:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_385_0.png)

图 10.32 – 更改 TargetGoal 的默认值

- 6. 编译、保存并关闭蓝图编辑器。
- 7. 要测试敌人的漫游行为，最好使用菜单中的 **Simulate** 选项，该选项可以通过单击 **Play** 按钮旁边的三个点来访问:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_386_0.png)

图 10.33 – 使用 Simulate 在关卡中自由移动

按住右键并使用 W、A、S、D 键和鼠标在关卡中自由移动。您将看到敌人生成并移动到关卡中的随机位置。

我们创建了一个自定义任务来查找随机漫游点，并修改了行为树以使用此新任务。现在，由 `BP_EnemySpawner` 生成的敌人知道如何在关卡中移动。

## 摘要

本章中，我们通过增强 AI 驱动敌人的能力，开始着手创造既具挑战性又平衡的游戏体验。我们赋予敌人类似僵尸的行为，让它们在关卡中无目的地漫游，直到通过视觉或听觉注意到玩家。我们还赋予它们在发现玩家时向前冲锋并发动近战攻击的能力，从而降低玩家的生命值。接着，我们给予玩家通过攻击敌人来反击的机会，最终在敌人生命值耗尽时将其消灭。最后，我们给予新

