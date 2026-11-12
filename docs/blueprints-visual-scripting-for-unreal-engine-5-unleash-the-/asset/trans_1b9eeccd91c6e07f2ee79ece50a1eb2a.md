

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_544_0.png)

图 16.4 – VR 模板中的用户表示

VRPawn 中的另一个组件是 TeleportTraceNiagaraSystem，它是一个用于表示瞬移痕迹的 Niagara 粒子系统组件。

然后，我们还有一对运动控制器组件。VR 模板使用这些额外组件作为获取瞄准位置的简单方式。默认情况下，运动控制器组件使用握把位置，但您可以在详细信息面板中更改运动源。下一张截图显示了 MotionControllerRightAim 的属性。显示设备模型属性未被勾选，因此此运动控制器组件不会在关卡中显示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_545_0.png)

图 16.5 – MotionControllerRightAim 的属性

最后要提及的组件是小部件交互组件。它的工作方式类似于激光指针，用于与小部件菜单交互，该菜单通过按下运动控制器上的菜单按钮激活：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_546_0.png)

图 16.6 – 与小部件菜单交互

在 **VRPawn** 的 EventGraph 中，有来自运动控制器的多个输入事件，我们将在 _瞬移_ 和 _对象抓取_ 节中详细介绍。

## 瞬移

在本节中，我们将了解用于瞬移的事件和函数。

要开始瞬移，请向上按下右侧运动控制器的摇杆以标记一个位置。当您释放摇杆时，将瞬移到标记的位置。瞬移目标由 **VRTeleportVisualizer** 蓝图表示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_547_0.png)

图 16.7 – VRTeleportVisualizer 蓝图

用于瞬移的输入事件是 **InputAxis MovementAxisRight_Y**。该事件的第一个节点检查 **Axis Value** 是否为正，这意味着摇杆已向上按下，并检查 **Axis Value** 是否大于死区，即启动瞬移所需的最小 **Axis Value**：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_547_1.png)

图 16.8 – InputAxis MovementAxisRight_Y 的第一个节点

下一张截图显示了连接到 *图 16.8* 中 **Branch** 节点的 **True** 输出的节点。**Do Once** 节点用于确保在瞬移痕迹激活期间，**Start Teleport Trace** 函数不会再次运行。**Teleport Trace** 函数会在用户向上按住摇杆时不断更新目标：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_548_0.png)

图 16.9 – 激活瞬移痕迹

下面的截图显示了连接到 *图 16.8* 中 **Branch** 节点的 **False** 输出的节点，这意味着用户没有向上按下右侧运动控制器的摇杆。如果瞬移痕迹处于活动状态，则该事件会结束瞬移痕迹并尝试瞬移到目标。**Try Teleport** 节点连接到 **Do Once** 节点的 **Reset** 引脚：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_548_1.png)

图 16.10 – 结束瞬移痕迹并瞬移

让我们分析用于瞬移的 **Start Teleport Trace**、**TeleportTrace**、**End Teleport Trace** 和 **Try Teleport** 函数。

**Start Teleport Trace** 函数在 **Teleport Trace Active** 布尔变量中设置 `True`，在 **Teleport Trace Niagara System** 组件中将可见性设置为 `True`，并生成 **VRTeleportVisualizer** 的一个实例：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_549_0.png)

图 16.11 – Start Teleport Trace 函数

TeleportTrace 函数使用名为 Predict Projectile Path By Object Type 的函数来计算投射瞬移位置和瞬移痕迹路径位置。下面的截图显示了 TeleportTrace 函数的最后几个节点，这些节点会更新 TeleportVisualizer 位置，并设置 Niagara 粒子系统组件用于瞬移痕迹视觉效果的向量数组：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_549_1.png)

图 16.12 – Teleport Trace 函数的最后节点

End Teleport Trace 函数与 Start Teleport Trace 函数相反。它将 Teleport Trace Active 布尔变量设置为 False，销毁瞬移可视化器实例，并隐藏瞬移痕迹 Niagara 系统组件：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_549_2.png)

图 16.13 – End Teleport Trace 函数

End Teleport Trace 函数使用一个 **Validated Get** 节点。当您拥有一个 **Object Reference Get** 节点时，可以右键单击它并选择 **Convert to Validated Get**：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_550_0.png)

图 16.14 – 将 Get 节点转换为 Validated Get 节点

原始节点被转换为一个带有执行引脚的 **GET** 节点，可用于检查对象引用是否有效：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_550_1.png)

图 16.15 – 带有分支执行引脚的 GET 节点

Try Teleport 函数检查 **Valid Teleport Location** 是否为 **True**，然后使用 **Teleport** 函数将用户瞬移到目标位置：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_551_0.png)

图 16.16 – Try Teleport 函数

VR 模板中还有另一种名为 **Snap Turn** 的运动类型。在这种运动中，您可以通过按下左侧运动控制器摇杆的左或右来旋转虚拟角色。Snap Turn 使用的输入事件是 **InputAxis MovementAxisLeft_X**。

我们了解了用于瞬移的事件和函数。在下一节中，我们将了解 VR 模板中对象抓取的工作原理。

## 对象抓取

对象抓取系统基于为 VR 模板创建的 **GrabComponent** 蓝图，该蓝图位于 `Content > VRTemplate > Blueprints` 文件夹中。

**GrabComponent** 是 **SceneComponent** 的子类。有关蓝图组件的更多信息，请参见 _第 18 章，创建蓝图库和组件_。

要使关卡中的任何 Actor 都可抓取，请将 **GrabComponent** 添加到 Actor 并将 **Mobility** 设置为 **Movable**：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_552_0.png)

图 16.17 – 使 Actor 可抓取

有一个名为 **Grab Type** 的枚举，用于定义对象如何附加到运动控制器。有关枚举的更多信息，请参见 _第 13 章，数据结构和流程控制_。

您可以在 **GrabComponent** 的 **详细信息** 面板中设置 **Grab Type**：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_552_1.png)

图 16.18 – Grab Type 枚举

可用的抓取类型如下：

- **None**：当您希望禁用抓取但又不移除 Actor 的 `Grab` 组件时，请使用此选项。
- **Free**：对象附加到运动控制器，同时保持与其相对的位置和方向。此类型最适合无需以特定方式握持即可使用的对象，例如 VR 模板中的立方体。
- **Snap**：拾取时，对象会附加到抓取它的运动控制器相对于的特定预定义位置和旋转。此抓取类型通常用于具有明确握把位置的对象，例如 VR 模板中的武器。
- **Custom**：此选项允许开发者创建自己的抓取类型。`Grab` 组件具有 `bIsHeld` 布尔变量以及 `OnGrabbed` 和 `OnDropped` 事件调度器，可用于创建自定义逻辑。

**VRPawn** 中处理抓取的事件是 **InputAction GrabLeft** 和 **InputAction GrabRight**，它们由运动控制器的握把按钮触发。要抓取关卡中的 Actor，请将运动控制器靠近 Actor 并按住握把按钮。要释放 Actor，请松开握把按钮。

让我们分析 **InputAction GrabLeft** 事件。下面的截图显示了该事件的第一个节点：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_553_0.png)

