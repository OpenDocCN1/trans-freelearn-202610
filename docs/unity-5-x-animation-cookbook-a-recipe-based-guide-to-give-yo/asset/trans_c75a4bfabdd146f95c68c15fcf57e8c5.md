
## Getting ready

我们需要几个存储面部表情的**混合形状**。在本教程中，我们将使用**微笑**、**愤怒**、**BlinkLeft**、**BlinkRight**和**BrowsDown**形状。在你的 3D 软件中创建它们。如果你使用 Blender，**混合形状**称为 Shape Keys。在其他软件中，它们可能被称为 Morph Targets。

你也可以使用提供的示例 Unity 项目并进入 `Chapter 05 Character actions and expressions\Recipe 08 Animating facial expressions with blend shapes` 目录。你会在那里找到一个 `Example.scene`。打开它，运行游戏，并调整 UI 上的滑块以查看效果。你可以在 `Rigs` 目录中找到带有所有混合形状的角色。

## How to do it...

要使用**混合形状**进行面部表情，请按照以下步骤操作：

- 1. 将你的角色导入 Unity。将其放置在场景中。
- 2. 在与角色相同的位置创建一个空游戏对象。将该对象命名为**FaceAnims**。
- 3. 将角色设为**FaceAnims**游戏对象的子对象。
- 4. 在选中**FaceAnims**游戏对象后，打开**动画**视图（前往**窗口** | **动画**）。
- 5. 创建一个新的动画剪辑并将其命名为`Angry`。
- 6. 点击添加属性按钮并选择**Character** | **Mesh** | **SkinnedMesh Renderer** | **Blend Shape.Angry**，其中**Character**是你的角色游戏对象的名称，**Mesh**是包含**蒙皮网格渲染器**组件的角色游戏对象的名称，而**Angry**是从 3D 软件导出的混合形状的名称。
- 7. 点击**Blend Shape.Angry**选项旁边的加号图标以添加该属性。
- 8. 进入**曲线**选项卡。选择两个关键帧并右键单击其中一个。选择**编辑键**选项并在**值**字段中输入 100。
- 9. 这样我们就创建了一个**愤怒**动画，其中**愤怒混合形状**设置为 100（最大值）。
- 10. 对**微笑**、**BlinkLeft**、**BlinkRight**和**BrowsDown**混合形状重复步骤 5 到 9。记住根据混合形状的名称命名动画（这样更容易识别动画）。
- 11. 为**FaceAnims**游戏对象自动创建了一个 Animator Controller。打开它。
- 12. 删除控制器中的所有状态并添加一个新的**混合树**。
- 13. 双击**混合树**以进入其设置。
- 14. 将**混合树**类型设置为**直接**。它允许将多个动画一起混合。
- 15. 在**混合树**中创建五个**动作字段**并将**Angry**、**Smile**、**BlinkLeft**、**BlinkRight**和**BrowsDown**动画分配给它们。
- 16. 在控制器中创建五个`float`参数，每个动画一个。相应地命名它们并将它们分配给**动作字段**（**Smile**参数分配给**Smile**动画，**Angry**参数分配给**Angry**动画，以此类推）。
- 17. 编写一个新脚本并将其命名为`BlendMultiple.cs`。在脚本中创建五个公共 float 成员变量并根据**混合形状**名称命名它们（`public float Smile`、`public float Angry`，以此类推）。
- 18. 在`Update()`函数中，我们将控制器参数的值设置为变量的值。这样当我们更改变量的值时，控制器中的参数也会更改，我们的角色会播放面部表情：
- 19. 将脚本附加到**FaceAnims**游戏对象。运行游戏并在运行时更改脚本的 float 变量以查看效果（你可以使用**场景**视图）。

```
anim.SetFloat("Angry", Angry);
anim.SetFloat("Smile", Smile);
anim.SetFloat("BlinkLeft", BlinkLeft);
anim.SetFloat("BlinkRight", BlinkRight);
anim.SetFloat("BrowsDown", BrowsDown);
```

![](img/616a75b593bef4212f618360fceea442_181_0.png)

混合不同混合形状会产生有趣的面部表情

## How it works...

本教程有几个关键要素使其发挥作用：

- **混合形状**：你需要从 3D 软件导出混合形状。混合形状存储同一网格的不同版本。网格必须具有相同数量的顶点。在创建新的混合形状时，你只能移动顶点（不能添加或删除顶点）。
- **FaceAnims 游戏对象**：我们的角色是新游戏对象**FaceAnims**的子对象。这使我们能够为面部表情设置动画。由于我们的角色拥有自己的 Animator 组件和自己的动画，因此我们无法使用**动画**视图对其进行修改。添加父游戏对象解决了这个问题。
- **直接混合树**：我们使用设置为**直接**的**混合树**。这允许我们同时混合多个动画。这适用于混合形状和面部表情。

## There's more...

你不必为混合形状创建动画剪辑。相反，你可以直接从代码中操作它们。在**蒙皮网格渲染器**组件上使用`setBlendShapeWeight()`函数。你需要知道**混合形状**在**混合形状**数组中的索引（你可以在检查器中，在角色的**蒙皮网格渲染器**组件中查看）。我们准备了一个设置权重的简短脚本。它附加到**DirectBlendShapes**游戏对象。你也可以在该教程的`Scripts`目录中找到它。

为**混合形状**创建动画可以提供更大的灵活性。你也可以使用**动画**视图为混合形状的权重设置动画。这样你就可以创建对话动画和逼真的动画面部表情。

## 6

## Handling Combat

本章解释了战斗中动画的使用并涵盖了以下主题：

- 在 Animator Controller 中使用子状态机
- 使用动画事件触发脚本函数
- 使用从任意状态的转换来播放受击反应
- 使用根运动创建闪避动作
- 检查当前活动的 Animator 状态以禁用或启用玩家操作
- 使用动画事件绘制武器
- 使用 Avatar Mask 和动画控制器层进行行走和瞄准
- 使用`LookAt()`方法进行瞄准
- 使用混合树进行瞄准
- 检测角色上的命中位置

## Introduction

每个游戏都不同，每个游戏都以自己的方式处理战斗机制。尽管如此，几乎在每种情况下都有一个通用的概念集合是有用的。在本章中，我们将介绍近战和远程战斗必不可少的工具。

## Using Sub-State Machines in Animator Controller

本教程展示了如何使用子状态机。这个概念有助于在 Animator Controller 中组织流程。

## Getting ready

开始时，你需要一个具有几个动画的角色。在此示例中，我们使用具有**NormalIdle**、**NormalMove**、**CombatIdle**和**CombatMove**动画的角色。这些动画随后将被分组到两个**子状态机：Combat**和**Normal**中。

你也可以打开提供的示例 Unity 项目并进入`Chapter 06\Handling combat\Recipe 01 Using sub state machines in animator controller`目录。在`Rigs`目录中你会找到所有必要的动画。在`Controllers`目录中，你可以找到带有两个子状态机的完成控制器。你可以在`Example.unity`场景中找到**Humanoid**角色。如果按下空格键，我们的角色将从**Normal**切换到**Combat 子状态机**。

## How to do it...

要在控制器中创建子状态机，请按照以下步骤操作：

- 1. 创建并打开一个新的 Animator Controller。
- 2. 右键单击空白区域并选择**创建子状态机**。将创建一个新的子状态机。将其命名为`Normal`。
- 3. 创建另一个子状态机并将其命名为`Combat`。
- 4. 创建一个`bool`参数：**CombatState**。
- 5. 创建两个转换：
  - **Combat** | **Normal** 无条件。
  - **Normal** | **Combat** 无条件。
- 6. 双击**Normal**子状态机。
- 7. 创建一个`float`**Speed**参数。
- 8. 将**NormalIdle**和**NormalMove**动画拖放到控制器中（确保你在**Normal**子状态机中）。
