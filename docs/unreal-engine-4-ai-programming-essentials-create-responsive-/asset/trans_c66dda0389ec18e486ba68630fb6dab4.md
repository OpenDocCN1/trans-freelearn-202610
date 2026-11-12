

注释此区域 清除任何值：

![](img/aaaa027482c900e215ba6bfab3c17337_142_0.png)

- 4. 接下来，在先前注释的名为 **Don't follow** 的部分之前，针对追随者 **作为领导者**，我们想在此处与 **ForEachLoop** 节点的 **Loop Body** 引脚之间添加间距。

- 5. 首先，我们需要计算 **NAlignment**，这是负责引导附近代理朝同一方向移动的行为。我们将获取附近代理的平均方向向量。

- 6. 现在，我们想要设置一个局部变量 **NAlignment**，方法是先从 **Cast to FlockingBall** 节点获取我们的 **Direction** 变量。现在，我们需要将 **Direction** 值乘以一个基于该代理与我们代理之间距离的强度。

- 7. 首先，我们需要计算我们与该代理之间的向量长度。我们将通过从其他代理的位置减去我们的位置来实现这一点。然后，我们将从结果中获取 **VectorLength** 值。接下来，我们将创建 **Map Range Clamped**，并将 **VectorLength** 节点的结果插入到 **Value** 引脚中。

- 8. 现在，要配置此 **Map Range Clamped** 节点，我们将 **In Range B** 设置为 **300.0**，然后将 **Out Range A** 设置为 **1.0**。这将导致在 0 单位时达到全强度，超过 300 单位则无强度。此返回值现在需要乘以 **Direction** 值。

- 9. 然后，我们将此结果添加到 **NAlignment**，并将其设置回局部 **NAlignment** 变量：

![](img/aaaa027482c900e215ba6bfab3c17337_143_0.png)

- 10. 现在，我们必须计算 Cohesion，这是负责引导代理朝附近代理中心移动的行为。这是指向代理质心的方向。

- 11. 首先，让我们创建一个 **GetActorLocation** (A) 节点和一个 **Lerp (vector)** 节点，将 **A** 引脚和来自重路由的 **GetActorLocation** (B) 节点连接起来。**Alpha** 值应为步骤 6 中 **Map Range Clamped** 的 **Return Value**。

- 12. 然后，我们想要将 **Lerp (vector)** 节点的 **Return Value** 添加到 **NCohesion**。接下来，我们将使用最终结果设置我们的局部 **NCohesion** 变量。这将为附近的每个代理更新我们的 **NCohesion** 变量：

![](img/aaaa027482c900e215ba6bfab3c17337_143_1.png)

- 13. 最后要计算的行为是 Separation，它负责将代理从附近的代理中推开。这是从我们的代理指向其他代理的 **Direction** 向量。

- 14. 首先，我们想要从我们的代理的 **GetActorLocation** 节点中减去 **GetActorLocation** 节点。然后，这将作为 **Lerp (vector)** 节点上的 B。我们将返回到 **Map Range Clamped** 节点的 **Return Value**，并将其插入到 **Lerp (vector)** 节点的 **Alpha** 引脚中。我们将留空 A。

- 15. 然后，我们必须将 **Lerp (vector)** 的 **Return Value** 添加到 **NSeparation**。最后，我们将使用结果设置我们的局部 **NSeparation** 变量。注释此区域 **Calculate Each Behavior for nearby Agents**:

![](img/aaaa027482c900e215ba6bfab3c17337_144_0.png)

- 16. 关注 **Loop Body** 变量中的 **Completed**。现在，我们必须完成三种行为的计算。

- 17. 让我们获取 **SphereOverlapActors** 返回的数组长度，以便知道除以多少来创建平均值。然后，我们将此 Int 转换为 Float。从那里，我们可以除以 **NAlignment** 变量，并使用 **Normalize** 节点对结果进行归一化。然后，我们必须获取在 **RollingGameMode** 中定义的全局变量。

- 18. 创建一个 **Get Game Mode** 节点并将其转换为 `RollingGameMode` 以访问我们的变量。接下来，我们将从 RollingGameMode 转换中获取 **Global Alignment** 变量，并将其乘以前一步的 **Normalize** 结果。然后，此结果必须设置到局部 **NAlignment** 变量中：

![](img/aaaa027482c900e215ba6bfab3c17337_145_0.png)

- 19. 接下来，我们必须计算 Cohesion。为此，我们必须将局部 **NCohesion** 变量除以数组长度，然后从我们的 **GetActorLocation** 节点中减去它。然后，我们将应用 **Normalize** 函数以获得我们需要的结果。

- 20. 接下来，我们将结果乘以来自 RollingGameMode 的 **Global Cohesion** 变量。然后，将此设置到我们的局部 **NCohesion** 变量中：

![](img/aaaa027482c900e215ba6bfab3c17337_145_1.png)

- 21. 类似于之前的过程，我们想要获取我们的局部 **NSeparation** 变量，并用之前的数组长度除以它。将结果乘以 -1。然后，将结果乘以全局 **Global Separation** 变量。

- 22. 最后，我们将获取结果并设置我们的局部 NSeparation 变量。注释此区域 Calculating Flocking Steering Behavior - outputs NCohesion, NAlignment, NSeparation:

![](img/aaaa027482c900e215ba6bfab3c17337_146_0.png)

- 23. 然后，我们必须找到 Calculate Direction，并将我们的每个变量添加到此代理的 Direction 变量的最终计算中：

![](img/aaaa027482c900e215ba6bfab3c17337_146_1.png)

- 24. 我们需要做的最后一件事是为本课程的下一部分做准备。所以，让我们在蓝图左上方找一个空位置并添加一些新代码。

- 25. 添加一个名为 ResetBall 的新自定义事件。

- 26. 接下来，我们想要为此代理创建一个 SetActorLocation 节点，并将 New Location 设置为我们在开始时设置的 Start Location 变量。

- 27. 现在，我们想要设置一个新方向，方法是首先放置 **SET** 节点的 **Direction** 变量。然后，我们将拆分结构体，并仅将 **Random Unit Vector** 节点的 **Return Value X** 和 **Return Value Y** 应用过去：

![](img/aaaa027482c900e215ba6bfab3c17337_147_0.png)

## 通过 UMG 控制行为

在本章的这部分中，我们想要涉及使用 UMG 来控制对我们的代理产生影响的行为。我们将通过首先创建一个带有适当控件的 UMG 小部件来操作我们的三个浮点变量来实现这一点。然后，我们必须将此用户小部件分配给拥有的 PlayerController。最后，我们将添加一个函数，将代理重置到其原始位置，重新开始模拟。

### 一个简单的 UI

让我们导航到内容浏览器，创建一个新的 Widget 蓝图，并将其命名为 `FlockingUI`。让我们打开它并转到 **Designer** 选项卡以开始，具体步骤如下：

- 1. 让我们将 **Vertical Box** 拖入我们的 **Hierarchy** 面板。
- 2. 接下来，设置插槽（**Canvas Panel**）属性：**Size X** 值为 350.0，**Size Y** 值为 600.0。
- 3. 然后，将一个按钮拖入 **Vertical Box** 下方的 **Hierarchy** 面板。将按钮重命名为 **ResetButton**。接下来，设置 **Vertical Box** 插槽的 **Padding** 属性为 75.0 和 25.0。

- 4. 最后，将 Text 拖入 ResetButton 下方的 Hierarchy 面板。接下来，设置文本属性 Text 为 "Reset":

![](img/aaaa027482c900e215ba6bfab3c17337_148_0.png)

- 5. 现在，让我们将 Horizontal Box 拖入 Vertical Box 下方的 Hierarchy 面板。然后，在 Horizontal Box 内添加两个更多小部件，分别名为 Text 和 Slider。

- 6. 现在，我们必须在 Hierarchy 面板中再复制两次 Horizontal Box 及其子项。

- 7. 右键单击并复制 Horizontal Box，然后将其粘贴到 Vertical Box 中两次：

![](img/aaaa027482c900e215ba6bfab3c17337_148_1.png)

- 8. 之后，我们想要重命名将用于组织目的的小部件。按顺序，我们想要将第一个滑块命名为 AlignmentSlider，第二个命名为 CohesionSlider，最后一个命名为 SeparationSlider。

- 9. 现在，按顺序，让我们将我们的文本属性 Text 设置为这三个名称：Alignment、Cohesion 和 Separation。然后，我们将设置 Padding 文本属性为 7.5。

完成此操作后，您应该会得到类似于以下截图的内容：

![](img/aaaa027482c900e215ba6bfab3c17337_149_0.png)

- 10. 要更新我们的全局变量，我们必须从三个滑块创建一个事件。我们可以从 AlignmentSlider 开始，然后转到 Events，并单击 OnValueChanged 上的 + 选项。然后，当此事件被调用时，我们将更新 RollingGameMode 中的 Global Alignment 变量。

- 11. 现在，我们必须对其他两个滑块——CohesionSlider 和 SeparationSlider——执行相同操作，使用相应的全局值：

![](img/aaaa027482c900e215ba6bfab3c17337_149_1.png)

