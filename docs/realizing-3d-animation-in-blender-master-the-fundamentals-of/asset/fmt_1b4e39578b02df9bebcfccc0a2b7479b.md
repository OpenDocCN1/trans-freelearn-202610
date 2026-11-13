
我们可以使用**眼球目标**来确定 Rain 眼球的朝向，而不必直接旋转眼球。这在 Rain 需要注视特定物体时，能让姿势摆设和动画制作变得更容易。

要了解其工作原理，让我们回到 **FK** 层中的一根骨骼：

1.  找到并选中 `FK-Head`。
2.  在全局 `Z` 轴上旋转 `FK-Head`，将 Rain 的头稍微转向一侧。

注意 Rain 的视线始终锁定在眼球目标骨骼上，导致她斜眼看我们：

![Figure 6.42: Rain stays looking at the eye target](image/Figure_06.42.0_B17149.jpg)

Figure 6.42: Rain stays looking at the eye target

至此，你已了解眼球目标骨骼的工作原理，甚至不需要移动它！只需确保这些骨骼始终位于 Rain 面部前方；否则，她最终会看向自己的后脑勺。

> **Tip**
>
> 如果不想在 Rain 头部移动时操心眼球目标骨骼，可以前往 `Face Settings` 面板，将 `Eye Target Parent` 改为 `Head`。该面板还允许你调整 Rain 眼球旋转对眼睑和周围皮肤的影响程度。

Rain 的姿势已近完成——剩下的只是摆放之前隐藏的配饰。

### 最后的配饰

取消隐藏 Rain 的头发和围巾后，我们发现还有一点工作要做：

![Figure 6.43: Rain’s scarf and ponytail, un-posed](image/Figure_06.43.0_B17149.jpg)

Figure 6.43: Rain’s scarf and ponytail, un-posed

头发和宽松衣物不像其他部位那样对角色姿势有贡献。它们没有自己的肌肉，只是垂在那里，对重力、Rain 的动作以及风做出反应。难怪我们想在完成其他所有姿势之前先隐藏这些部分。

让我们把配饰还给 Rain，并让它们自然下垂：

1.  取消隐藏 `Hair` 和 `Clothes` 骨架层，并在 `Outfits` 面板中启用 `Scarf`、`Hair Strand` 和 `Hair Ponytail`。
2.  摆放 Rain 的马尾，使其看起来自然下垂。从头部向外旋转红色的 `FK` 骨骼开始，然后使用球形拉伸骨骼微调结果。
3.  用相同方法摆放 Rain 的围巾（本例中发丝可保持不动）。

现在 Rain 的站立姿势完成了。比她最初的 `T` 姿势好多了！

![Figure 6.44: The finished standing pose](image/Figure_06.44.0_B17149.jpg)

Figure 6.44: The finished standing pose

虽然我们的第一个姿势表面上看很简单，但即使是这一个也花了一些时间，并要求我们利用 Rain 的许多功能。随着练习的深入，给角色摆姿势会变得越来越容易！

话虽如此，让我们之前的工作白费可不划算。为此，我将展示如何保存和复用我们制作的任何姿势，以便在后续动画中使用。

# 创建姿势库

角色动画本质上只是一系列经过精炼的姿势，因此通过创建更多姿势进行练习，我们能学到很多。传达动作、重量、力量、张力、思考和情感的姿势，都是制作引人入胜的动画的要素。而且因为制作一个姿势可能需要一段时间，你可能想在后续动画中复用其中一些！

让我们学习如何保存姿势，以便保留当前姿势供后续练习使用。

## 资产浏览器

除了模型和材质，Blender 的新 `Asset Browser` 还能存储我们为角色制作的姿势。

现在打开它。为了自动为每个姿势创建缩略图，我们还需要在场景中添加一个摄像机：

1.  切换到 `Object Mode`，按 `Shift` + `A` 添加一个 `Camera` 对象到场景中。
2.  将摄像机对准 Rain。
3.  在新区域中打开 `Asset Browser` 区域。
4.  在左侧 `Source List`（`T`）区域中，将 `Asset Library` 从 `All` 改为 `Current File`。
5.  点击 `+` 创建一个 `New Asset Catalog` 项。
6.  将其命名为 `Rain Poses`。

现在我们有了一个空目录：

![Figure 6.45: Catalog in the Asset Browser (currently empty)](image/Figure_06.45.0_B17149.jpg)

Figure 6.45: Catalog in the Asset Browser (currently empty)

我们制作的任何喜欢的姿势都可以存在这里。让我们从已制作的站立姿势开始。

## 创建姿势资产

要将姿势存储为资产，请按以下步骤操作：

1.  选中姿势涉及的所有骨骼。
2.  在 `Asset Browser` 区域中，点击 `Asset` | `Create Pose Asset`。
3.  在 `Asset Browser` 区域右侧边栏（`N`）中更新 `Name` 属性，给姿势起一个更具描述性的名称。

点击 `Create Pose Asset` 时，所选骨骼所在的位置，就是该姿势日后应用时的位置。

“姿势涉及的所有骨骼”指我们希望在将该姿势应用到新项目中的 Rain 时受影响的所有骨骼。这包括我们摆过姿势的所有骨骼，但也可能（或不妨）包括一些未摆姿势的骨骼。对于当前的站立姿势，我们可以直接选中已用图层中的所有骨骼。对于更局部的姿势，可以使用更窄的骨骼选择。例如，仅选面部骨骼做表情，或仅选手指骨骼做“手部姿势”（如握拳）。

> **Important note**
>
> 存储 `CloudRig` 属性（如 `IK/FK Switch`）的骨骼不在可见图层上，因此不会作为姿势的一部分被保存。我们得自己记得调整这些设置。

试一下。这是我们在 `Asset Browser` 区域中的第一个姿势：

![Figure 6.46: The standing pose saved as an asset](image/Figure_06.46.0_B17149.jpg)

Figure 6.46: The standing pose saved as an asset

> **Tip**
>
> 你可以随时在右侧边栏（`N`）的 `Preview` 面板中更新资产的缩略图。

保存当前姿势后，我们准备制作另一个！你可以选中所有摆过姿势的骨骼，使用 `Pose` | `Clear Transform` | `All` 将 Rain 重置为原始休息姿势；或者，如果你愿意，也可以基于现有姿势在下一节中制作新姿势。

## 练习姿势

我们不再按部就班地摆姿势了；你现在对骨架图层、`CloudRig` 功能以及在姿势模式下操控骨骼已有足够了解，可以自行创建更多姿势。每次创建一个喜欢的姿势，都可以用 `Create Pose Asset` 存入 `Asset Browser` 区域。

> **Tip**
>
> 摆姿势时以自己的身体为参考；离开电脑，试着做出你想重现的姿势！有时镜子对面部表情很有帮助。

### 更多简单姿势

尝试制作任意数量的常见姿势。如果保存下来，说不定日后在动画中会用得上。

#### 站立

站立不止一种方式。基于刚才制作的现有姿势，我们可以探索脚、手臂和脊柱的替代位置：

![Figure 6.47: Standing poses – leaning, arms crossed, hands folded](image/Figure_06.47.0_B17149.jpg)

Figure 6.47: Standing poses – leaning, arms crossed, hands folded

接下来，我们尝试对 Rain 容易但对我们稍具挑战的事：坐着！

#### 坐着和/或躺着

试着用立方体作为椅子和桌子的简单参考。对于完全躺卧的姿势，可以对所有肢体使用 `FK`：

![Figure 6.48: Reclined poses](image/Figure_06.48.0_B17149.jpg)

Figure 6.48: Reclined poses

坐着或睡着的角色与地面和/或家具有更多接触点。所以具有讽刺意味的是，角色越放松，我们要做的工作越多。

现在让我们看看在面部表情上能有多大创意。

### 面部表情

以下是几个可尝试的面部表情示例。记住，先尽可能利用 `Face Primary` 图层中的黄色骨骼，再用其他骨骼微调表情：
