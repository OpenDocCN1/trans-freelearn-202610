
正如我们在前几章中制作动画的物体一样，骨架中的所有骨骼都具有位置、旋转和缩放属性。直接变换一个或多个骨骼被称为**姿态设定**，这是角色动画的第一步。在某一时间点，骨架中骨骼的给定位置组合在一起，被称为一个**姿态**。在后续章节为 Rain 制作动画时，我们会让她摆出一个姿态，为刚设定姿态的那些骨骼的属性插入关键帧，然后在另一帧进入下一个姿态，为那些骨骼的属性插入关键帧，以此类推。

这就是角色动画。它与物体动画并没有太大区别！只不过这次我们的物体是骨骼，数量有成百上千个，而且它们连接成一个复杂的系统，乍看之下可能不太直观。简单，对吧？也许并不。在为 Rain 制作动画之前，我们最好花一两节的时间专门学习如何为她设定姿态。

我们的第一个练习将让 Rain 从 *T* 姿态过渡到一个闲置的、自然的站立姿势：

![Figure 6.19: Resting “T pose” to idle standing pose](image/Figure_06.19.0_B17149.jpg)

Figure 6.19: 静止“T 姿态”到闲置站立姿态

如果想让它看起来自然，即使是简单的站立姿态也需要操纵大量骨骼。

## 姿态模式

要开始为 Rain 设定姿态，请选择她的骨架，并在 3D 视口中进入 `Pose Mode`（`Ctrl` + `Tab`）：

![Figure 6.20: Pose Mode in the Interaction Mode menu](image/Figure_06.20.0_B17149.jpg)

Figure 6.20: 交互模式菜单中的姿态模式

在 `Pose Mode` 中，骨骼的选择和变换方式与 `Object Mode` 中的普通物体非常相似，使用相同的标准控制进行 `Move`（`G`）、`Rotate`（`R`）和 `Scale`（`S`）操作。

在 `Pose Mode` 中为骨架设定姿态，在技术上不同于绑定师所做的在 `Edit Mode` 中编辑骨架。我们无法编辑链接骨架物体中的骨骼，在制作动画时也不应该需要这样做。

**重要提示**

在 `Pose Mode` 下，你无法添加或选择其他物体；必须返回 `Object Mode` 才能进行这些操作。

由于 Rain 的控制骨骼具有复杂的关系，且不会直接变形其网格，因此变换某个特定骨骼的结果并不总是可预测的。虽然某个骨骼的形状及其在 Rain 身体上的位置能提供关于该骨骼功能的线索，但了解骨骼功能的唯一可靠方法是移动它并观察会发生什么。

**提示**

不要害怕移动某个骨骼来查看其功能。你随时可以 `Undo`（`Ctrl` + `Z`），或在确认变换前使用右键或 `Esc` 取消变换。此外，在下一节中，我将向你展示如何重置在 `Pose Mode` 下对骨骼所做的任何更改。

### 设定躯干和臀部的姿态

我们从哪根骨骼开始？在最初为角色设定姿态时，通常最好从控制躯干整体位置的骨骼开始。对于 Rain 来说，这是 `MSTR-Pelvis_Parent`。如果你难以找到它，它是 Rain 臀部周围那根四叶草形状的骨骼。

**提示**

要一目了然地查看 Rain 的骨骼名称，请前往骨架的 `Object Data Properties` 区域，找到 `Viewport Display` 面板，并启用 `Show` | `Names`。稍后当我们取消隐藏更多骨架层时，你可能想要禁用此选项。

我们将使用 `MSTR-Pelvis_Parent` 给 Rain 的腿留出一点弯曲空间：

1.  进入 `Pose Mode` 并选择 `MSTR-Pelvis_Parent`。
2.  沿全局 *Z* 轴向下移动 `MSTR-Pelvis_Parent` 约 **-0.02** 米。

现在让我们使用名为 `MSTR-Hips` 的马鞍形骨骼将 Rain 的臀部向右摆动：

1.  选择 `MSTR-Hips`。
2.  沿全局 *Y* 轴旋转 `MSTR-Hips` 约 **10** 度。

仅仅对这两根骨骼进行这些细微的改动，Rain 看起来就更放松、更自然了。她开始不再像一个模特人台，而更像一个真实的人！

![Figure 6.21: Subtle difference in pose](image/Figure_06.21.0_B17149.jpg)

Figure 6.21: 姿态的细微差别

请注意，`MSTR-Pelvis` 和 `MSTR-Hips` 都会移动 Rain 的臀部，但其中一个是另一个的子骨骼，影响范围更局部。这个极端的例子展示了两者的区别：

![Figure 6.22: Making Rain squat with MSTR-Pelivs versus MSTR-Hips](image/Figure_06.22.0_B17149.jpg)

Figure 6.22: 使用 MSTR-Pelvis 与 MSTR-Hips 让 Rain 下蹲的对比

骨骼能动并不总意味着它就该动！话虽如此，你大可放心尝试。在姿态模式下，对骨骼的所有更改都是可逆的，没有意外删除或破坏任何东西的危险。如果有骨骼弯曲得太过分，只需记住以下操作符来重置它……

### “取消姿态”已设定姿态的骨骼

每根骨骼都有一个**休息位置**——即在我们于 `Pose Mode` 中变换它之前的初始位置。设定完骨骼姿态后，我们稍后经常想将它们重置为休息位置。这对于在进入下一个姿态前“放松”骨骼使其脱离当前姿态很有用，以确保 Rain 不会带着任何轻微脱臼的关节四处走动，或者只是在胡闹一番后恢复理智……

![Figure 6.23: There’s an easy way to fix this](image/Figure_06.23.0_B17149.jpg)

Figure 6.23: 有简单的方法可以修复这个问题

在前面的例子中，我把 `MSTR-Pelvis` 放在了一个荒谬的位置。要修复这个问题，无需尝试手动把 `MSTR-Pelvis` 放回原处，也无需数值化地重置其所有变换数值。我只需选择有问题的骨骼，然后执行 `Pose` | `Clear Transform`：

![Figure 6.24: Clear Transform operators](image/Figure_06.24.0_B17149.jpg)

Figure 6.24: 清除变换操作符

使用 `Pose` | `Clear Transform` | `All` 可将 `MSTR-Pelvis` 恢复到初始位置，从该位置重新开始设定姿态会更容易。

**提示**

除了 `Pose` | `Clear Transform` | `All`，三个主要的 `Pose` | `Transform` 操作符（`Move`（`G`）、`Rotate`（`R`）和 `Scale`（`S`））各自都有对应的 `Pose` | `Clear Transform` 操作符：`Location`（`Alt` + `G`）、`Rotation`（`Alt` + `R`）和 `Scale`（`Alt` + `S`）。它们的工作方式与 `Object Mode` 中的 `Object` | `Clear` 操作符相同，但在角色动画中特别有用。

既然我们知道如何快速重置任何骨骼，在设定姿态时尝试各种操作就毫无顾虑了。我们在 `Pose Mode` 中所做的任何更改，无论多么荒谬或扭曲，都可以撤销。

## IK 骨骼

看看 Rain 的脚，注意当我们移动她的臀部和躯干时，她的脚保持静止，导致膝盖弯曲。目前，Rain 的脚由 `MSTR-Foot_Parent.L` 和 `MSTR-Foot_Parent.R` 骨骼控制，它们默认独立于我们之前移动的那些骨骼。

**提示**

因为人和生物在 *X* 轴上往往是对称的，所以许多骨架骨骼成镜像对出现——一根用于左侧，一根用于右侧。这些对中的骨骼名称相同，仅后缀为 *L* 或 *R* 有所不同。

让我们看看移动左脚骨骼会发生什么：

1.  选择 `MSTR-Foot_Parent.L`（这是 Rain 左脚鞋周围那根鞋形骨骼）。
2.  沿全局 *Z* 轴向外旋转该骨骼约 **20** 度。
3.  沿全局 *X* 轴向 Rain 的左侧移动该骨骼约 **0.2** 米。
4.  沿全局 *Y* 轴向后移动该骨骼约 **0.1** 米。

再加上几处不对称调整，就构成了 Rain 姿态那种放松的外观：

![Figure 6.25: A casual, natural-looking leg stance](image/Figure_06.25.0_B17149.jpg)

Figure 6.25: 休闲、自然的腿部站姿
