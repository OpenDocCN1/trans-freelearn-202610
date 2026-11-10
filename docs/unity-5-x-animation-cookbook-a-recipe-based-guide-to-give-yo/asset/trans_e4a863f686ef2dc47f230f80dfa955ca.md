
9. 关闭 Animator Controller 并将其分配给角色的 Animator 组件。

10. 创建一个新脚本并将其命名为 EdgeGrab.cs（你可以在本教程的 Scripts 目录中找到完成的脚本）。此脚本的目标是在我们触碰悬崖边缘时播放适当的动画。我们还需要禁用碰撞和物理，并确保在播放动画时角色位于正确的位置（在 RootTarget 游戏对象的位置）。

11. 首先我们需要一个 Grab() 函数来处理玩家开始触碰边缘的情况（我们稍后将从 GrabTrigger 游戏对象调用此函数）：

```
public void Grab (Transform target) {
    grabTarget = target;

    if (grabTarget != null)
    {
        anim.SetTrigger("GrabEdge");
        adjustPosition = true;
        steeringScript.enabled = false;
        jumpScript.enabled = false;
        rb.isKinematic = true;
        anim.applyRootMotion = true;
    }
}
```

12. 此函数有一个 target 参数，将保存对 RootTarget 游戏对象的引用。我们将此参数赋给类成员变量 grabTarget（我们稍后会用到它）。我们通过在控制器中设置 GrabEdge Trigger 来开始播放 GrabEdge 动画。我们还将一个标志（类成员）adjustPosition 设置为 true。此标志稍后用于确定是否应将角色的位置与 RootTarget 游戏对象的位置匹配。然后我们禁用存储在 steeringScript 和 jumpScript 变量中的两个脚本。我们这样做只是因为我们不希望它们干扰我们的边缘抓取动作。我们还将 RigidBody 组件（rb 变量存储对其的引用）的 isKinematic 选项设置为禁用碰撞和重力。最后，我们为 Animator 组件（引用存储在 anim 变量中）启用 root motion。这使得 GrabEdgeClimb 动画能够正常工作。

13. 接下来我们需要编写 GrabLerp() 函数来处理角色位置与 RootTarget 位置的匹配：

```
void GrabLerp()
{
    if (grabTarget == null || !adjustPosition)
    {
        return;
    }
    if ((transform.position -
    grabTarget.
```

## 5 角色动作和表情

本章解释了角色动作和表情动画的使用方法，并涵盖了以下教程：

- 创建出现或消失动画
- 创建具有动画驱动行为的背景角色和小动物
- 使用 **Blend Trees** 创建随机动作
- 使用 `Quaternion.LookRotation()` 和 `Animator.SetLookAtPosition()` 方法使角色用视线跟随物体
- Action Points – 在指定位置执行动作
- 将动画与场景中的对象同步
- 使用 IK 与场景对象交互
- 使用 Blend Shapes 动画化面部表情

## 简介

在大多数游戏中，角色会执行各种动作和表情。本章解释了如何使用动画来创建逼真的行为和交互。

## 创建出现或消失动画

如果我们的游戏中有怪物，那么我们很可能需要使用出现或消失动画来在场景中引入这些角色。经常情况下，怪物会在玩家附近生成以伏击他们。这时出现动画就派上用场了。

## 准备工作

开始之前，你应该有一个至少包含两种动画的角色：循环的 **Idle** 动画和一个 **Appear** 动画。此动画应从地面下方开始（如果你的角色是飞行类型的，则在空中高处）。它应以 **Idle** 姿势结束。参考下图：

![](img/616a75b593bef4212f618360fceea442_157_0.png)

出现动画关键帧（动画是“原地”完成的）

你可以下载提供的示例；在 Unity 中打开项目并进入 `Chapter 05 Character actions and expressions\Recipe 01 Creating an appear or disappear animation` 文件夹。你会在那里找到一个名为 `Example.Scene` 的场景。播放时，一个 Spider 角色将被生成（实例化）并播放 **Appear** 动画。在 `Rigs` 目录中，你可以找到包含所需动画的 `Spider.fbx` 资源。

## 如何操作...

要创建出现动画，请按照以下步骤操作：

1. 导入带有 **Idle** 和 **Appear** 动画的动画角色。
2. 确保勾选 **Idle** 动画的 **Loop Time** 选项。
3. 将导入的角色放置在场景中。
4. 创建一个新的 Animator Controller 并将其命名为 `Appear`（或者你可以编辑现有的控制器）。
5. 将你的 **Appear** 动画拖放到控制器中并确保它是默认状态。如果不是，右键单击它并选择 **Set As Layer Default State** 选项。
6. 将你的 **Idle** 动画拖放到控制器中。
7. 创建一个过渡：
   - **Appear** | **Idle** 无条件过渡：**Has Exit Time** 应设置为 true，**Transition Duration** 设置为 0.2 秒。
8. 将 Animator Controller 分配给场景中站立的角色（分配到其 Animator 组件）。
9. 为了更好地看到 **Appear** 动画的效果，从角色创建一个预制体。为此，将其从 **Hierarchy** 拖放到 **Project** 视图中的任意文件夹。
10. 从 **Hierarchy** 中删除角色并编写一个脚本在运行时生成（实例化）角色。在本教程中，我们使用示例 Unity 项目 `Shared Scripts` 目录中的 `Spawner.cs` 脚本。
11. 此脚本在与 **Spawner** 游戏对象（`Spawner.cs` 脚本附加到的对象）相同的位置和旋转下生成预制体（预制体列表）。

## 原理...

我们在这里使用一个非常简单的机制——当 Unity 实例化一个动画游戏对象时，它总是播放该游戏对象的默认动画状态。因此，当我们将 **Appear** 动画设置为默认状态时，角色被生成（实例化）时就会播放它。这样我们就可以创建有趣的动画，使角色出现在玩家附近。在此示例中，我们的角色从地面爬出。

## 还有更多...

为了使效果完整，你还应为角色添加一个生成 VFX。你可以在提供的 Unity 项目的 `Prefabs` 目录中找到一个示例：名为 **RocksEffect** 的粒子系统预制体。这是一个“挖掘出土”效果。我们使用相同的 `Spawner.cs` 脚本生成它。此外，此预制体还有一个 `AutoDestroy.cs` 脚本，可在几秒后销毁生成的特效。

## 创建具有动画驱动行为的背景角色和小动物

在本教程中，我们将创建动画角色作为游戏中的装饰物。此类角色的行为仅由动画驱动；因此，我们可以在游戏中放置相当多的此类角色。

## 准备工作

开始之前，我们需要一个具有几种动画的角色。在此示例中，我们使用一只鸟。它具有 **Idle**、**StartFlying**、**FlyingInCircles** 和 **Land** 动画。**Idle** 动画是循环的，当我们的鸟坐在地面上时播放。**StartFlying** 动画是 **Idle** 和 **FlyingInCircles** 之间的过渡，后者是鸟飞行的循环动画。最后一个动画 **Land** 动画是 **FlyingInCircles** 和 **Idle** 之间的过渡。我们不为这些动画使用 root motion，因为我们希望鸟总是落在同一位置（我们不想检查地面在哪里，这只是装饰）。

你也可以下载提供的示例 Unity 项目并进入 `Chapter 05 Character actions and expressions\Recipe 02 Creating background characters and critters with animation driven behavior` 目录。你会在那里找到一个名为 `Example.scene` 的场景，其中有一些鸟在周围坐着。按下空格键，鸟会开始飞行。再按一次，它们会降落。在 `Rigs` 目录中，你可以找到具有所有所需动画的 `Bird.fbx` 角色。
