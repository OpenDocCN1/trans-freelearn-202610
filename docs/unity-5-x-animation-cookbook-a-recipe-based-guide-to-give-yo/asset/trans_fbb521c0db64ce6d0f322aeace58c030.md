
13. 我们需要为角色编写一个脚本，以便我们的死亡陷阱能够伤害它们。创建一个新的 C# 脚本并将其命名为 Character.cs。编写以下代码：

```
using UnityEngine;
using System.Collections;

public class Character : MonoBehaviour
{
    /*We are going to store the reference to a blood effect prefab
    in this variable*/
    public GameObject bloodEffect;

    /*This variable is set to true when the character object was
    already killed*/
    bool isKilled = false;

    /*This function is called by the death
    trap, when we enter it*/
    public void Kill ()
    {
        /*If the character was already killed by the trap,
        we don't want to do anything*/
        if (isKilled) {
            return;
        }
        /*If it was not killed, we set the isKilled
        variable to true*/
        isKilled = true;

        /*We check if the character has a RigidBody component*/
        Rigidbody rb = GetComponent<Rigidbody> ();

        if (rb != null) {
            /*If we find the component, we need to set it to
            kinematic to prevent our character from being
            launched in the air by the collision with our
            trap*/
            rb.isKinematic = true;
        }

        /*Here we spawn a blood effect prefab stored in the
        bloodEffect variable*/
        GameObject.Instantiate (bloodEffect, transform.position
            + Vector3.up * 2f, Quaternion.identity);

        /*We are getting all the Renderer components of our character*/
        Renderer[] r = GetComponentsInChildren<Renderer> ();

        for (int i = 0; i < r.Length; i++) {
            /*We are turning all the renderers off, making the object disappear*/
            r [i].enabled = false;
        }

        /*We are also checking if our character uses our SimpleMove script if so, we are turning it off to prevent player from moving the character after death*/
        SimpleMove move = GetComponent<SimpleMove> ();

        if (move != null) {
            move.enabled = false;
        }
    }
}
```

- 14. 选择 **左陷阱** 游戏对象并为其添加一个 **盒形碰撞器** 组件。
- 15. 调整碰撞体形状，使其不会覆盖尖刺部分（我们计划在那里添加一个触发器）。为此，请在 **盒形碰撞器** 组件中点击 **编辑形状** 按钮。形状手柄将出现在 **场景视图** 中 **盒形碰撞器** 的每个面上。您可以点击手柄并移动它来调整形状。
- 16. 为 **左陷阱** 游戏对象添加一个 **刚体** 组件并将其设置为 **是否运动学**。
- 17. 右键点击 **左陷阱** 游戏对象并选择 **3D 对象 | 立方体** 为其添加一个立方体子对象。它将成为我们的触发器。

18. 将立方体缩放以覆盖尖刺。参见以下截图：

![](img/616a75b593bef4212f618360fceea442_83_0.png)

19. 从立方体中移除网格渲染器和网格过滤器组件。为立方体指定一个描述性的名称，例如 LeftTrapTrigger。

20. 在 LeftTrapTrigger 的盒形碰撞器中勾选是否触发选项。

21. 对右陷阱重复步骤 14-20。

22. 现在我们需要为死亡陷阱本身编写一个脚本。创建一个新的 C# 脚本并将其命名为 DeathTrap.cs。打开脚本并编写以下代码：

```
using UnityEngine;
using System.Collections;

public class DeathTrap : MonoBehaviour
{
    /*This function is called when a RigidBody enters the trigger object*/
    void OnTriggerEnter (Collider other)
    {
        /*We are checking if the object which entered the trigger has a Character script, if so we are calling the Kill() method on it*/
        Character characterScript = other.gameObject.GetComponent<Character> ();

        if (characterScript != null) {
            characterScript.Kill ();
        }
    }
}
```

- 23. 将 `DeathTrap.cs` 脚本附加到 **左陷阱触发器** 游戏对象和 **右陷阱触发器** 游戏对象。
- 24. 确保你的角色具有一个 **刚体** 组件
（Unity 标准资源中的 **第三人称角色** 预制件可用）。
- 25. 将 `Character.cs` 脚本添加到你的角色。
- 26. 创建一个血粒子效果并将其保存为预制件。将其命名为 **血花**。
- 27. 将 **血花** 预制件分配给角色中 **角色** 脚本组件的 **血效果** 字段。
- 28. 播放游戏并进入陷阱以查看效果。你的角色应该消失并且 **血花** 效果应该被生成。

## 它是如何工作的...

在本教程中，我们将触发器（设置为 **是否触发** 的 **盒形碰撞器** 组件的游戏对象）作为陷阱移动部件的子对象附加。只要具有 **刚体** 组件的对象进入我们的触发器对象之一，我们的 `DeathTrap.cs` 脚本中的 `OnTriggerEnter()` 方法就会被调用。对象或触发器是否在移动并不重要。陷阱移动部件的动画也会移动子触发器对象。您可以使用它在游戏中创建一些非常壮观的陷阱。

## 更多内容...

在我们的示例中，只要角色触碰到陷阱的尖刺就会死亡。我们可以通过使用动画事件在陷阱关闭时启用触发器，在陷阱打开时禁用它们来增加更多真实感。这将防止我们在陷阱没有关闭时杀死角色。您可以在 `Chapter 6, Handling Combat` 中了解有关动画事件的更多信息。

请记住，您也可以旋转触发器，就像我们在 *使用层级动画本地旋转 – 创建一个环绕的行星* 教程中所做的那样。缩放触发器也是可能的。

您不应将此概念用于移动普通碰撞器。当您想要移动碰撞器时，始终尝试使用设置为 **是否运动学** 的刚体（出于优化原因）。

## 创建一个由玩家输入触发的电梯

在本最后一个教程中，我们将使用玩家输入创建一个简单的电梯。

### 准备工作

开始之前，您应该准备一个电梯模型来包含对象：名为 **电梯** 的根对象、**升降平台** 对象（这个将被动画化），以及用作装饰的 **电梯框架** 对象。**升降平台** 和 **电梯框架** 对象应为空的 **电梯** 对象的子对象。您也可以使用提供的示例 Unity 项目并进入 `Chapter 02 Working with the animation view\Recipe 07 Creating an elevator triggered by player input` 目录。您将在那里找到一个 `Example.unity` 场景，其中包含 **电梯** 游戏对象及其子对象 **升降平台** 和 **电梯框架**。示例资源中的 **电梯** 已被动画化。

### 如何操作...

要创建由玩家输入触发的电梯，请按照以下步骤操作：

- 1. 选择根 **电梯** 对象并为其分配一个动画器组件。将 **更新模式** 设置为 **动画物理**。
- 2. 打开 **窗口 | 动画** 并通过点击 **创建** 按钮创建一个新的动画剪辑。将其命名为 **ElevatorGoingDown**。
- 3. 确保录制按钮被按下（在 **动画视图** 的左上角）。
- 4. 选择你的 **升降平台** 游戏对象并将其移动到上位置。
- 5. 将时间线向前调整几秒并将 **升降平台** 游戏对象移动到下位置。你的 **升降平台** 游戏对象应从最大上位置动画到最小下位置。
- 6. 通过从动画下拉列表中选择 **创建新剪辑** 选项创建另一个动画剪辑（您可以在播放按钮下方找到它）。将其命名为 **ElevatorGoingUp**。
- 7. 将你的 **升降平台** 游戏对象从下位置动画到上位置。
- 8. 创建另一个动画剪辑并将其命名为 **ElevatorDown**。为 **升降平台** 游戏对象在下位置创建一个一秒循环动画。
- 9. 创建另一个动画剪辑并将其命名为 **ElevatorUp**。为 **升降平台** 游戏对象在上位置创建一个一秒循环动画。
- 10. 关闭 **动画** 视图。
- 11. 在 **项目** 视图中导航到 **ElevatorGoingUp** 和 **ElevatorGoingDown** 资源。选择它们并在 **检查器** 中取消勾选 **循环时间** 选项。
- 12. 在 **层级** 中选择 **电梯** 游戏对象并在 **检查器** 中找到其动画器组件。
- 13. 双击 **控制器** 字段以打开自动创建的动画控制器资源。
- 14. 创建一个新的触发器参数并将其命名为 `Move`。
- 15. 创建一个转换循环，如以下截图所示：

![](img/616a75b593bef4212f618360fceea442_86_0.png)

- 16. 确保 **ElevatorDown** 状态是默认状态。如果不是，右键点击它并选择 **设置为图层默认状态** 选项。
- 17. **ElevatorDown | ElevatorGoingUp** 和 **ElevatorUp | ElevatorGoingDown** 转换都应将 **Move** 触发器设置为条件并将 **Has Exit** 选项设置为 false。
- 18. **ElevatorGoingDown | ElevatorDown** 和 **ElevatorGoingUp | ElevatorUp** 转换应将 **Has Exit Time** 选项设置为 true 并且不应有其他条件。

