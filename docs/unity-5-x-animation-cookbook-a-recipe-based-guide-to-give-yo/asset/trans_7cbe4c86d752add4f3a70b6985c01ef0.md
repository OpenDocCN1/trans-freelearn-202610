
- **过渡偏移**：这会在时间上偏移目标动画。值以动画的百分比表示。将其设置为 0.3 意味着目标动画将从其长度的 30% 开始，而不是从开头开始。
- **中断源**：此设置告诉 Unity 该过渡是否可以被其他过渡中断。你可以将其设置为：
  - **无**：过渡不能被任何其他过渡中断。
  - **当前状态**：过渡可以被我们尝试从中过渡的状态的过渡中断。
  - **下一个状态**：过渡可以被我们尝试过渡到的状态的过渡中断。
  - **当前状态然后下一个状态**：过渡可以被我们尝试从中过渡的状态的过渡或我们尝试过渡到的状态的过渡中断。从我们尝试从中过渡的状态的过渡具有优先级。
  - **下一个状态然后当前状态**：过渡可以被我们尝试从中过渡的状态的过渡或我们尝试过渡到的状态的过渡中断。从我们尝试过渡到的状态的过渡具有优先级。
  - **有序中断**：如果设置为 false，则允许过渡被其他过渡独立于其顺序地中断。如果设置为 true，则过渡的顺序很重要。

## 还有更多...

当你创建 Animator Controller 时，除了动画状态外，还可以看到另外三个节点：

- **入口和出口**：此节点用于在状态机（Animator Controllers 或子状态机）之间过渡时。我们将在第 6 章《处理战斗》中的《在 Animator Controller 中使用子状态机》配方中详细介绍它。
- **任意状态**：此节点可作为辅助手段，用于从任意状态向给定状态进行过渡。当你有一个可以随时播放的动画时使用，例如受击动画。我们将在第 6 章《处理战斗》中的《使用从任意状态的过渡来播放受击反应》配方中详细介绍它。

## 使用参数控制动画流程

你可以在 Animator Controller 中定义一组参数，并使用它们来控制 Mecanim 中动画状态之间的过渡。在本配方中，我们将展示如何使用参数作为过渡条件，并使用脚本在运行时设置这些参数的值。

## 准备工作

在开始之前，你应该准备一个 Animator Controller，其中至少有一个动画状态之间的过渡。控制器应分配给放置在场景中的角色（其 Animator 组件）。你也可以使用提供的示例 Unity 项目并进入 Chapter 01 Working with animations\Recipe 05 Using parameters to control the animation flow 目录。你将在那里找到一个 Example.unity 场景。场景的 Hierarchy 中有一个 Warrior 游戏对象。如果运行游戏并按下空格键，Warrior 将做出挥手手势。你可以选择 Warrior 并打开其 Animator Controller。如果点击 Idle | Wave 过渡，你将能够看到过渡条件。

## 如何操作...

要使用参数控制状态过渡，请按照以下步骤操作：

- 1. 打开 Animator Controller 资源。
- 2. 在 Animator 窗口左上角找到 Parameters 选项卡并点击它。
- 3. 在 Parameters 选项卡中点击加号图标以添加新参数。
- 4. 为参数选择 Trigger 类型。
- 5. 输入新创建参数的名称（在提供的示例中，参数的名称是 Wave）。
- 6. 点击你想要使用参数的状态之间的过渡。在提供的示例中，它是 Idle 和 Wave 动画状态之间的过渡。Idle 是默认状态。
- 7. 进入 Inspector 选项卡并找到 Conditions 部分。
- 8. 点击加号图标添加新条件。如果只有一个参数，它将自动被选为条件。如果有多个参数，你需要从下拉列表中选择合适的参数。
- 9. 如果你想在动画状态之间立即过渡，请确保禁用 Settings 折叠面板上方的 Has Exit Time 选项。
- 10. 只有当其条件满足时，过渡才会发生。你需要使用脚本设置参数。
- 11. 要创建新的 C# 脚本，在 Project 视图中右键单击并选择 Create | C# Script。按你喜欢的名称命名脚本（在提供的示例中，它称为 Wave，与其设置的参数相同）。
- 12. 打开脚本并写入以下内容：

![](img/616a75b593bef4212f618360fceea442_45_0.png)

```csharp
using UnityEngine;
using System.Collections;

public class Wave : MonoBehaviour {
//The anim variable is used to store the reference
//to the Animator component of the character.
private Animator anim;
void Start () {
//We get the component and assign it to
//the anim variable when the game starts
anim = GetComponent<Animator>();
}
void Update () {
//We check if player pressed the spacebar
if (Input.GetKeyDown(KeyCode.Space))
{
/*We call the SetTrigger() function on the
Animator component stored in the anim
variable. The function requires one
parameter - the name of the trigger
parameter set in our Animator Controller
("Wave" in our example). Make sure to match
it with the name of the parameter you've
created in your Animator Controller*/
anim.SetTrigger("Wave");
}
}
}
```

- 13. 确保你的类名与脚本的文件名相同，否则无法编译。
- 14. 将脚本分配给角色，分配到具有 Animator 组件且附加了你的 Animator Controller 的同一个 Transform 上。播放游戏并按空格键；你应该看到你的角色切换到下一个动画状态。

## 工作原理...

你可以使用几种类型的参数以及相应的脚本函数来设置它们：

- **Trigger**：这是最简单的参数。它通过在 Animator 组件对象上调用 SetTrigger(string name) 函数设置为 true。它在被过渡消耗（使用）后由 Animator Controller 重置。函数的 string name 参数必须与你在 Animator Controller 中设置的触发器参数名称匹配。
- **Int**：这是一个整数参数。当你使用它时，你需要在条件中指定一个逻辑比较。过渡只有在参数的值满足与给定数字的比较条件时才会发生。你可以使用 Equal、Greater、Less 和 Not Equal 选项来比较参数的值与给定数字。整数类型参数使用 SetInteger(string name, int value) 函数设置。string name 参数需要与控制器中设置的参数名称匹配。int value 参数是设置为控制器参数的值。
- **Float**：这是一个浮点参数。它与整数类型的工作方式相同，但使用浮点数而不是整数。它使用 SetFloat(string name, float value) 函数设置。
- **Bool**：这是一个布尔参数。条件可以检查参数是否为 true 或 false。参数的值使用 `SetBool(string name, bool value)` 函数设置。

## 还有更多...

- 你可以在过渡 **Inspector** 中的 **Conditions** 部分点击加号图标为状态过渡添加多个条件。要使过渡发生，所有条件都必须满足。它对条件的工作方式相当于逻辑 *与*。
- 你也可以在同一状态之间创建多个过渡。为此，右键单击你想要从中过渡的状态并选择 **Make Transition** 选项，然后选择你已有的过渡目标状态。多重过渡用三个箭头而不是一个箭头标记。如果任一过渡的条件满足，过渡就会发生。你可以将其用作过渡条件的逻辑 *或*。
- 如果状态之间有多个过渡，你一次只能编辑其中一个。要编辑过渡，请在 **Inspector** 的 **Transitions** 部分中选择它。

![](img/616a75b593bef4212f618360fceea442_47_0.png)

- 如果你想删除一个过渡，在 **Inspector** 中选择它并点击减号图标，或在 **Animator Controller** 中选择它并按键盘上的 *删除*。按 *删除* 会删除所有过渡。

