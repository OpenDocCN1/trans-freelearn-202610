

## 更高级的移动

在本章中，我们将专注于群体行为和更高级的路径跟随行为。我们要实现的是实现群体行为，为我们的 AI 创建真实的移动效果，例如当你需要智能体在同一方向移动时相互避让。有时，你的智能体需要寻找领导者，以便创建不同的智能体编队。首先，我们要设置所需的一切，让一些 pawn 在关卡中四处移动。接下来，我们将为这些 pawn 添加一些蓝图，使它们能够发现新的领导者。最后，我们将介绍群体行为，以便我们可以看到我们的 AI 作为一个群体如何移动。

本章涵盖的主题如下：

- 设置用于移动的 actor 蓝图
- 实现跟随行为
- 实现具有分离、聚合和对齐等特性的群体行为
- 通过 UMG 添加行为控制

### 设置智能体

让我们使用 Physics Ball 模板创建一个名为 AdvancedMovement 的新项目。当前，我们使用的是 Unreal Engine 4.8.3。我们首先要做的是找到 PhysicsBallBP 类并打开 EventGraph。我们可以按照以下要点操作：

- 1. 项目加载后，进入 RollingBP 部分，然后进入 Blueprints 文件夹。
- 2. 在这里，你会找到 PhysicsBallBP，它将在本章中充当我们的智能体。让我们为这个 actor 打开 EventGraph。

> 我们要引入两个新的向量变量来保存智能体的当前方向。另一个将保存智能体生成的位置。

- 3. 现在，让我们从示例中删除任何不必要的逻辑。我删除了以下变量：
- **JumpImpulse** 变量
- **CanJump** 变量

EventGraph 中的所有蓝图代码均未显示在以下截图中：

![](img/aaaa027482c900e215ba6bfab3c17337_130_0.png)

- 4. 好了，现在我们有了基础，让我们创建一个名为 **Direction** 的变量，并将其类型设置为向量变量。

- 5. 接下来，创建一个名为 **StartLocation** 的变量，同样将其设置为向量变量类型。

> **StartLocation** 将在课程后面用于我们需要实现的 **Reset** 按钮。

- 6. 现在，找到 **Event Begin Play**，并初始化我们的新向量变量。首先，在事件附近的区域右键单击并找到 **Random Unit Vector**。从 **Random Unit Vector** 节点拆分向量（通过右键单击引脚），因为我们只需要 **X** 和 **Y** 值。

> 我们只能向前或向后以及向左或向右移动。这些增量由 X 和 Y 处理。如果引入 Z，可能会使摄像机向前旋转，这对于观察可能并不理想。

- 7. 接下来，让我们右键单击并创建一个名为 **Get Actor Location** 的新节点。我们需要拉下 **StartLocation** 向量设置变量。然后，从 **Return Value** 设置我们的 **Start Location** 变量。注释 **Give random direction and save start location** 区域：

![](img/aaaa027482c900e215ba6bfab3c17337_131_0.png)

- 8. 现在，我们需要将 **Direction** 变量设置为 **SpringArm** 变量的方向。这样做的原因是确保摄像机始终朝向我们在世界中移动的方向。因此，当你修改不同的变量时，可以看到它生效。

- 9. 将我们的 **SpringArm** 变量拖入 EventGraph。接下来，从 **SetRelativeRotation** 变量拉出连线。

- 10. 然后我们要获取 **SpringArm** 变量的当前 **RelativeRotation** 值，以便在我们应当朝向的方向之间进行插值。这样在更新摄像机旋转时会产生平滑过渡。

- 11. 通过从 **SpringArm** 变量的当前 **RelativeRotation** 引脚拉出，我们要创建 **RInterp To** 节点。
- 12. 接下来，我们要获取 **Direction** 变量，并使用 **Make Rot from X** 将此方向转换为旋转。
- 13. 我们将通过一个偏移量更改此旋转器。我将其计算为弹簧的本地旋转偏移量。在 **A** 中放入方向后，我们应该将 **B** 设置为 **0.0 Roll**、**45.0 Pitch** 和 **90.0 Yaw**。
- 14. 现在，将 **Return Value** 引脚连接到我们的 **RInterp To** 节点的 **Target** 引脚。
- 15. 现在，右键单击，搜索 **Get World Delta Seconds**，并将其插入到我们的 **RInterp To** 节点的 **Delta Time** 引脚中。
- 16. **Return Value** 引脚现在必须插入到我们先前创建的 **SetRelativeRotation** 节点的 **New Rotation** 中。

> 因此，我们要在此处更新方向。这样做的原因是
> 我们将继续在此操作中添加内容，以归一化我们的方向
> 并获得我们预期范围内的值。

将此区域注释为 **Calculate Direction for Camera Spring Arm**：

![](img/aaaa027482c900e215ba6bfab3c17337_132_0.png)

- 17. 为此，让我们拉下我们的 **Direction** 变量。从变量拉出并查找 Vector + Vector。从此处，我们要归一化向量。最后，让我们拆分向量。

- 18. 接下来，让我们拉出 **Direction** 变量并创建一个设置变量。让我们使用 **Break Vector** 拆分 **Direction** 设置变量。然后，将 **X** 和 **Y** 浮点变量都插入到 SET 节点的 **Direction X** 和 **Direction Y** 变量中。将此区域注释为 **Calculate Direction**：

![](img/aaaa027482c900e215ba6bfab3c17337_133_0.png)

- 19. 我们需要拉出一个 **Ball** 变量并调用名为 **Add Torque** 的节点。
- 20. 要计算 **Torque** 值，我们要获取 **Direction** 变量。获取 **Roll Torque** 变量并将其乘以 **Direction** 向量。结果应插入到 **Add Torque** 节点的 **Torque** 引脚中。将此区域注释为 **Apply Torque to Ball**：

![](img/aaaa027482c900e215ba6bfab3c17337_133_1.png)

- 21. 下一步是创建一个函数，用于扫描 **Ball** 变量前方。如果射线命中，则意味着我们击中了一面墙。从此处，我们要使用 **Hit Result** 基于 **Hit Normal** 生成墙面反射。让我们看看实际操作。

- 22. 从我们上一个节点开始，创建一个名为 **LineTraceByChannel** 的新节点。从此处，我们要通过右键单击并搜索 **Get Actor Location** 来获取一个新节点。将 **Return Value** 插入到我们新建节点的 **Start** 引脚中。

- 23. 获取 **Direction** 变量的另一个副本并将其放置在图表中。从我们的 **Direction** 向量拉出并右键单击创建一个名为 **Make Rot from Y** 的新节点。从 **Make Rot from Y** 的 **Return Value** 右键单击创建一个名为 **Get Up Vector** 的新节点。

- 24. 获取我们的 **Ball** 向量变量，然后右键单击创建一个名为 **Get LocalBounds** 的新节点。现在，拆分 Max 向量，并从 **Max X** 右键单击创建一个新节点 **Float * Float**。

- 25. 现在，此变量将获取我们的大小，并用它来确定球体外部的前向距离。在我们的示例中，我们将按 **-3.5** 进行缩放。我们将反转缩放，因为我们要乘的向量相对于球体，而不是世界。

- 26. 在 **Float * Float** 节点中，让我们将另一个浮点数设为 **-3.5**，它未连接。

- 27. 从我们的 **Get Up Vector** 节点开始，将其与前一步的 **Float * Float** 结果相乘。现在，我们需要右键单击并找到一个 **Vector + Vector** 节点。将此向量添加到我们的 **GetActorLocation** 节点。然后，将结果插入到我们的 **LineTraceByChannel** 节点的 **End** 引脚中：

![](img/aaaa027482c900e215ba6bfab3c17337_134_0.png)

- 28. 从我们的 **LineTraceByChannel** 节点拆分命中结果。现在，找到 **Out Hit Blocking Hit** 并从布尔结果创建一个分支。让我们拉出我们的 **Direction** 向量并获取 SET 变量。这应该连接到先前创建的 **Branch** 节点的 **True** 输出引脚。

- 29. 从我们的 **Out Hit Normal** 引脚，右键单击并创建一个 **Make Rot from Y** 节点。现在，我们要从该返回值拉出，右键单击并创建一个 **Get Up Vector** 节点。这将世界中的法线转换为相对方向空间。最后，我们要获取 **Direction** 向量以获取变量。

- 30. 从我们的 **Direction** 变量拉出并释放以查找 **Mirror Vector by Normal**。让我们将前一步的 **Get Up Vector** 节点插入到 **Mirror Vector by Normal** 的 **In Normal** 中。此节点的结果固定在在 **True** 语句中执行的 **SET** 变量的 **Direction** 引脚上。

将此区域注释为 **Bounce current direction off wall closing in front of ball**：

![](img/aaaa027482c900e215ba6bfab3c17337_135_0.png)

Bounce current direction off wall 的完整蓝图设置

现在我们的 **PhysicsBallBP** 类或智能体已更新以处理方向向量，我们将用随机值对其进行初始化。它应持续向前移动，并在 actor 靠近足够撞击时反弹墙壁。

