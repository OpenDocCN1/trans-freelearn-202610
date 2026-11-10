
## 准备工作

为了制作一个动画门，你应该准备好两个对象：**门框** 和 **门**。**门** 应该是 **层级** 中 **框架** 的子对象。它应放置在 *关闭位置*。我们假设打开时我们的 **门** 会向上滑动。你还应该有一个分配了 `Player` 标签的玩家角色。角色应使用 **刚体** 组件移动或具有运动学 **刚体** 组件。我们将为门使用触发器，而触发器仅对刚体做出反应。你也可以前往 `Chapter 02 Working with the animation view\Recipe 04 Animating objects local position - creating automatic doors` 目录，并在其中找到 `Example.unity` 场景。如果打开它，你可以在 **层级** 中找到 **自动门** 游戏对象，以及一个将作为我们玩家使用的 **羊** 游戏对象（它具有 `Player` 标签）。如果运行游戏并靠近门，门会打开。

## 如何操作...

要创建自动门，请按照以下步骤操作：

- 1. 选择你的 **门框** 游戏对象，并为其添加 Animator 组件。为此，在 **检查器** 中点击 **添加组件** 按钮并选择 **Animator**。确保将组件添加到 **门框** 而不是 **门**（父对象而不是子对象）。将 **Animator** 组件的 **更新模式** 设置为 **动画物理**。
- 2. 在选择 **门框** 游戏对象的情况下，转到 **窗口 | 动画** 以打开 **动画** 视图。
- 3. 创建一个新的动画剪辑并将其命名为 `Closed`；这将是我们门的关闭动画。要创建新剪辑，点击回放按钮下方的下拉列表并选择 **创建新剪辑** 选项。
- 4. 点击 **添加属性** 按钮并在列表中找到你的 **门** 对象（**门框** 游戏对象的子对象）。
- 5. 展开 **门** 游戏对象折叠面板并找到 **变换** 部分。展开它并点击 **位置** 属性旁的加号图标，如以下屏幕截图所示：

![](img/616a75b593bef4212f618360fceea442_71_0.png)

- 6. 将为 **门** 游戏对象的本地位置添加两个关键帧。我们不会调整它们（假设 **门** 处于关闭位置）。
- 7. 添加另一个动画剪辑并将其命名为门打开。
- 8. 点击 **添加属性** 按钮，选择你的门游戏对象，并以步骤 5 相同的方式添加 **变换 | 位置** 关键帧。
- 9. 将第二个关键帧移动到大约 3 秒的位置。
- 10. 选择 **门** 游戏对象（**门框** 的子对象）。
- 11. 将其向上移动到打开位置。确保录制按钮处于活动状态（在 **动画** 视图的左上角）。如果播放动画，它应该将 **门** 游戏对象向上移动。
- 12. 选择 **门** 打开动画的最后一帧（在 **动画** 视图中）。转到 **检查器** 并复制门的 **变换** 组件。为此，点击组件右上角的小齿轮图标并选择 **复制组件** 选项。
- 13. 创建一个新的动画剪辑并将其命名为 `Opened`。以步骤 5 相同的方式为 **门** 子对象添加关键帧。如果你的 **门** 对象移动了，选择它，点击 **检查器** 中 **变换** 组件旁的齿轮图标，并选择 **粘贴组件值**。这将粘贴我们从 `Door Opening` 动画最后一帧复制的 **变换** 属性。
- 14. 确保动画中的两个帧具有相同的[content]
## 准备工作

为了制作一个动画门，你应该准备好两个对象：**门框** 和 **门**。**门** 应该是 **层级** 中 **框架** 的子对象。它应放置在 *关闭位置*。我们假设打开时我们的 **门** 会向上滑动。你还应该有一个分配了 `Player` 标签的玩家角色。角色应使用 **刚体** 组件移动或具有运动学 **刚体** 组件。我们将为门使用触发器，而触发器仅对刚体做出反应。你也可以前往 `Chapter 02 Working with the animation view\Recipe 04 Animating objects local position - creating automatic doors` 目录，并在其中找到 `Example.unity` 场景。如果打开它，你可以在 **层级** 中找到 **自动门** 游戏对象，以及一个将作为我们玩家使用的 **羊** 游戏对象（它具有 `Player` 标签）。如果运行游戏并靠近门，门会打开。

## 如何操作...

要创建自动门，请按照以下步骤操作：

- 1. 选择你的 **门框** 游戏对象，并为其添加 Animator 组件。为此，在 **检查器** 中点击 **添加组件** 按钮并选择 **Animator**。确保将组件添加到 **门框** 而不是 **门**（父对象而不是子对象）。将 **Animator** 组件的 **更新模式** 设置为 **动画物理**。
- 2. 在选择 **门框** 游戏对象的情况下，转到 **窗口 | 动画** 以打开 **动画** 视图。
- 3. 创建一个新的动画剪辑并将其命名为 `Closed`；这将是我们门的关闭动画。要创建新剪辑，点击回放按钮下方的下拉列表并选择 **创建新剪辑** 选项。
- 4. 点击 **添加属性** 按钮并在列表中找到你的 **门** 对象（**门框** 游戏对象的子对象）。
- 5. 展开 **门** 游戏对象折叠面板并找到 **变换** 部分。展开它并点击 **位置** 属性旁的加号图标，如以下屏幕截图所示：

![](img/616a75b593bef4212f618360fceea442_71_0.png)

- 6. 将为 **门** 游戏对象的本地位置添加两个关键帧。我们不会调整它们（假设 **门** 处于关闭位置）。
- 7. 添加另一个动画剪辑并将其命名为门打开。
- 8. 点击 **添加属性** 按钮，选择你的门游戏对象，并以步骤 5 相同的方式添加 **变换 | 位置** 关键帧。
- 9. 将第二个关键帧移动到大约 3 秒的位置。
- 10. 选择 **门** 游戏对象（**门框** 的子对象）。
- 11. 将其向上移动到打开位置。确保录制按钮处于活动状态（在 **动画** 视图的左上角）。如果播放动画，它应该将 **门** 游戏对象向上移动。
- 12. 选择 **门** 打开动画的最后一帧（在 **动画** 视图中）。转到 **检查器** 并复制门的 **变换** 组件。为此，点击组件右上角的小齿轮图标并选择 **复制组件** 选项。
- 13. 创建一个新的动画剪辑并将其命名为 `Opened`。以步骤 5 相同的方式为 **门** 子对象添加关键帧。如果你的 **门** 对象移动了，选择它，点击 **检查器** 中 **变换** 组件旁的齿轮图标，并选择 **粘贴组件值**。这将粘贴我们从 `Door Opening` 动画最后一帧复制的 **变换** 属性。
- 14. 确保动画中的两个帧具有相同的[content]
## 准备工作

为了制作一个动画门，你应该准备好两个对象：**门框** 和 **门**。**门** 应该是 **层级** 中 **框架** 的子对象。它应放置在 *关闭位置*。我们假设打开时我们的 **门** 会向上滑动。你还应该有一个分配了 `Player` 标签的玩家角色。角色应使用 **刚体** 组件移动或具有运动学 **刚体** 组件。我们将为门使用触发器，而触发器仅对刚体做出反应。你也可以前往 `Chapter 02 Working with the animation view\Recipe 04 Animating objects local position - creating automatic doors` 目录，并在其中找到 `Example.unity` 场景。如果打开它，你可以在 **层级** 中找到 **自动门** 游戏对象，以及一个将作为我们玩家使用的 **羊** 游戏对象（它具有 `Player` 标签）。如果运行游戏并靠近门，门会打开。

## 如何操作...

要创建自动门，请按照以下步骤操作：

- 1. 选择你的 **门框** 游戏对象，并为其添加 Animator 组件。为此，在 **检查器** 中点击 **添加组件** 按钮并选择 **Animator**。确保将组件添加到 **门框** 而不是 **门**（父对象而不是子对象）。将 **Animator** 组件的 **更新模式** 设置为 **动画物理**。
- 2. 在选择 **门框** 游戏对象的情况下，转到 **窗口 | 动画** 以打开 **动画** 视图。
- 3. 创建一个新的动画剪辑并将其命名为 `Closed`；这将是我们门的关闭动画。要创建新剪辑，点击回放按钮下方的下拉列表并选择 **创建新剪辑** 选项。
- 4. 点击 **添加属性** 按钮并在列表中找到你的 **门** 对象（**门框** 游戏对象的子对象）。
- 5. 展开 **门** 游戏对象折叠面板并找到 **变换** 部分。展开它并点击 **位置** 属性旁的加号图标，如以下屏幕截图所示：

![](img/616a75b593bef4212f618360fceea442_71_0.png)

- 6. 将为 **门** 游戏对象的本地位置添加两个关键帧。我们不会调整它们（假设 **门** 处于关闭位置）。
- 7. 添加另一个动画剪辑并将其命名为门打开。
- 8. 点击 **添加属性** 按钮，选择你的门游戏对象，并以步骤 5 相同的方式添加 **变换 | 位置** 关键帧。
- 9. 将第二个关键帧移动到大约 3 秒的位置。
- 10. 选择 **门** 游戏对象（**门框** 的子对象）。
- 11. 将其向上移动到打开位置。确保录制按钮处于活动状态（在 **动画** 视图的左上角）。如果播放动画，它应该将 **门** 游戏对象向上移动。
- 12. 选择 **门** 打开动画的最后一帧（在 **动画** 视图中）。转到 **检查器** 并复制门的 **变换** 组件。为此，点击组件右上角的小齿轮图标并选择 **复制组件** 选项。
- 13. 创建一个新的动画剪辑并将其命名为 `Opened`。以步骤 5 相同的方式为 **门** 子对象添加关键帧。如果你的 **门** 对象移动了，选择它，点击 **检查器** 中 **变换** 组件旁的齿轮图标，并选择 **粘贴组件值**。这将粘贴我们从 `Door Opening` 动画最后一帧复制的 **变换** 属性。
- 14. 确保动画中的两个帧具有相同的[content]
## 准备工作

为了制作一个动画门，你应该准备好两个对象：**门框** 和 **门**。**门** 应该是 **层级** 中 **框架** 的子对象。它应放置在 *关闭位置*。我们假设打开时我们的 **门** 会向上滑动。你还应该有一个分配了 `Player` 标签的玩家角色。角色应使用 **刚体** 组件移动或具有运动学 **刚体** 组件。我们将为门使用触发器，而触发器仅对刚体做出反应。你也可以前往 `Chapter 02 Working with the animation view\Recipe 04 Animating objects local position - creating automatic doors` 目录，并在其中找到 `Example.unity` 场景。如果打开它，你可以在 **层级** 中找到 **自动门** 游戏对象，以及一个将作为我们玩家使用的 **羊** 游戏对象（它具有 `Player` 标签）。如果运行游戏并靠近门，门会打开。

## 如何操作...

要创建自动门，请按照以下步骤操作：

- 1. 选择你的 **门框** 游戏对象，并为其添加 Animator 组件。为此，在 **检查器** 中点击 **添加组件** 按钮并选择 **Animator**。确保将组件添加到 **门框** 而不是 **门**（父对象而不是子对象）。将 **Animator** 组件的 **更新模式** 设置为 **动画物理**。
- 2. 在选择 **门框** 游戏对象的情况下，转到 **窗口 | 动画** 以打开 **动画** 视图。
- 3. 创建一个新的动画剪辑并将其命名为 `Closed`；这将是我们门的关闭动画。要创建新剪辑，点击回放按钮下方的下拉列表并选择 **创建新剪辑** 选项。
- 4. 点击 **添加属性** 按钮并在列表中找到你的 **门** 对象（**门框** 游戏对象的子对象）。
- 5. 展开 **门** 游戏对象折叠面板并找到 **变换** 部分。展开它并点击 **位置** 属性旁的加号图标，如以下屏幕截图所示：

![](img/616a75b593bef4212f618360fceea442_71_0.png)

- 6. 将为 **门** 游戏对象的本地位置添加两个关键帧。我们不会调整它们（假设 **门** 处于关闭位置）。
- 7. 添加另一个动画剪辑并将其命名为门打开。
- 8. 点击 **添加属性** 按钮，选择你的门游戏对象，并以步骤 5 相同的方式添加 **变换 | 位置** 关键帧。
- 9. 将第二个关键帧移动到大约 3 秒的位置。
- 10. 选择 **门** 游戏对象（**门框** 的子对象）。
- 11. 将其向上移动到打开位置。确保录制按钮处于活动状态（在 **动画** 视图的左上角）。如果播放动画，它应该将 **门** 游戏对象向上移动。
- 12. 选择 **门** 打开动画的最后一帧（在 **动画** 视图中）。转到 **检查器** 并复制门的 **变换** 组件。为此，点击组件右上角的小齿轮图标并选择 **复制组件** 选项。
- 13. 创建一个新的动画剪辑并将其命名为 `Opened`。以步骤 5 相同的方式为 **门** 子对象添加关键帧。如果你的 **门** 对象移动了，选择它，点击 **检查器** 中 **变换** 组件旁的齿轮图标，并选择 **粘贴组件值**。这将粘贴我们从 `Door Opening` 动画最后一帧复制的 **变换** 属性。
- 14. 确保动画中的两个帧具有相同的[content]
## 准备工作

为了制作一个动画门，你应该准备好两个对象：**门框** 和 **门**。**门** 应该是 **层级** 中 **框架** 的子对象。它应放置在 *关闭位置*。我们假设打开时我们的 **门** 会向上滑动。你还应该有一个分配了 `Player` 标签的玩家角色。角色应使用 **刚体** 组件移动或具有运动学 **刚体** 组件。我们将为门使用触发器，而触发器仅对刚体做出反应。你也可以前往 `Chapter 02 Working with the animation view\Recipe 04 Animating objects local position - creating automatic doors` 目录，并在其中找到 `Example.unity` 场景。如果打开它，你可以在 **层级** 中找到 **自动门** 游戏对象，以及一个将作为我们玩家使用的 **羊** 游戏对象（它具有 `Player` 标签）。如果运行游戏并靠近门，门会打开。

## 如何操作...

要创建自动门，请按照以下步骤操作：

- 1. 选择你的 **门框** 游戏对象，并为其添加 Animator 组件。为此，在 **检查器** 中点击 **添加组件** 按钮并选择 **Animator**。确保将组件添加到 **门框** 而不是 **门**（父对象而不是子对象）。将 **Animator** 组件的 **更新模式** 设置为 **动画物理**。
- 2. 在选择 **门框** 游戏对象的情况下，转到 **窗口 | 动画** 以打开 **动画** 视图。
- 3. 创建一个新的动画剪辑并将其命名为 `Closed`；这将是我们门的关闭动画。要创建新剪辑，点击回放按钮下方的下拉列表并选择 **创建新剪辑** 选项。
- 4. 点击 **添加属性** 按钮并在列表中找到你的 **门** 对象（**门框** 游戏对象的子对象）。
- 5. 展开 **门** 游戏对象折叠面板并找到 **变换** 部分。展开它并点击 **位置** 属性旁的加号图标，如以下屏幕截图所示：

![](img/616a75b593bef4212f618360fceea442_71_0.png)

- 6. 将为 **门** 游戏对象的本地位置添加两个关键帧。我们不会调整它们（假设 **门** 处于关闭位置）。
- 7. 添加另一个动画剪辑并将其命名为门打开。
- 8. 点击 **添加属性** 按钮，选择你的门游戏对象，并以步骤 5 相同的方式添加 **变换 | 位置** 关键帧。
- 9. 将第二个关键帧移动到大约 3 秒的位置。
- 10. 选择 **门** 游戏对象（**门框** 的子对象）。
- 11. 将其向上移动到打开位置。确保录制按钮处于活动状态（在 **动画** 视图的左上角）。如果播放动画，它应该将 **门** 游戏对象向上移动。
- 12. 选择 **门** 打开动画的最后一帧（在 **动画** 视图中）。转到 **检查器** 并复制门的 **变换** 组件。为此，点击组件右上角的小齿轮图标并选择 **复制组件** 选项。
- 13. 创建一个新的动画剪辑并将其命名为 `Opened`。以步骤 5 相同的方式为 **门** 子对象添加关键帧。如果你的 **门** 对象移动了，选择它，点击 **检查器** 中 **变换** 组件旁的齿轮图标，并选择 **粘贴组件值**。这将粘贴我们从 `Door Opening` 动画最后一帧复制的 **变换** 属性。
- 14. 确保动画中的两个帧具有相同的位置。
- 15. 创建一个额外的动画剪辑（最后一个）并将其命名为 `Door Closing`。
- 16. 使 **门** 游戏对象的动画从打开位置滑向关闭位置。记住你可以从适当的动画帧复制门的 **变换** 组件并将其粘贴到正在工作的动画中。
- 17. 使用 **动画** 视图预览所有动画。你可以使用回放按钮下方的下拉列表在动画之间切换。
- 18. 如果你对动画满意，关闭 **动画** 视图。选择你的 **门框** 游戏对象并在 **检查器** 中找到其 Animator 组件。
- 19. 点击 **控制器** 字段，这将在 **项目** 视图中显示自动创建的 Animator Controller。双击打开它。
- 20. 创建一个 `bool` 参数并将其命名为 `Open`。
- 21. 创建动画状态之间的转换循环：**Closed** | **Door Opening** | **Open** | **Door Closing** | **Closed**。请参阅以下屏幕截图：

![](img/616a75b593bef4212f618360fceea442_72_0.png)

- 22. 选择 **Closed** | **Door Opening** 转换。为其添加一个条件，选择 `Open` 参数，并将条件设置为 true。在转换中禁用 **Has Exit Time** 选项。
- 23. 选择 **Opened | Door Closing** 转换。为其添加一个条件，选择 `Open` 参数，并将条件设置为 false。在转换中禁用 **Has Exit Time** 选项。
- 24. 让 **Door Opening | Opened** 和 **Door Closing | Closed** 转换不带条件。确保在这两个转换中启用 **Has Exit Time** 选项。
- 25. 关闭 Animator Controller。
- 26. 在场景的层级中选择门框游戏对象。
- 27. 在门框游戏对象下添加一个 **立方体** 子对象。为此，右键点击 **门框** 游戏对象并选择 **创建 | 3D 对象 | 立方体**。这将是我们触发器。
- 28. 选择 **立方体** 并缩放它，使其在门框两侧突出。请参阅以下屏幕截图作为参考：

![](img/616a75b593bef4212f618360fceea442_73_0.png)

- 29. 从 **立方体** 中移除 **网格渲染器** 和 **网格过滤器** 组件。为此，点击给定组件右上角的小齿轮图标并选择 **移除组件** 选项。这将只在立方体上留下 **变换** 和 **盒状碰撞体** 组件。
- 30. 在立方体的 **盒状碰撞体** 组件上启用 **Is Trigger** 复选框。
- 31. 将立方体游戏对象重命名为 `Trigger`。
- 32. 选择 **门框** 游戏对象并为其添加 **网格碰撞体** 组件。
- 33. 选择 **门** 游戏对象并为其添加 **盒状碰撞体** 组件（或设置为 **凸** 的 **网格碰撞体**）以及一个 **刚体** 组件。在 **刚体** 组件中启用 **Is Kinematic** 选项。
- 34. 创建一个新的 C# 脚本并将其命名为 `AutomaticDoors.cs`。打开它并写入以下代码：

```csharp
using UnityEngine;
using System.Collections;

public class AutomaticDoor : MonoBehaviour
{
    /*The anim variable is used to store the reference
    to the Animator component*/

    private Animator anim;

    void Start ()
    {
        /*We assign the Animator component of the parent object
        because this script is attached to the trigger, which
        is the child object of our animated doors*/

        anim = transform.parent.GetComponent<Animator> ();
    }
    /* This function is called when a Rigidbody intersects with
    the collider attached to our game object for the first time.
    Our collider has to be set to trigger. The Collider other
    parameter stores information about the object which collided
    with our trigger (entered the trigger).*/
    void OnTriggerEnter (Collider other)
    {

        //We check the tag of the object entering the trigger
        if (other.gameObject.CompareTag ("Player")) {

            /*If the tag equals "Player", we set the
            bool parameter "Open" to true in our
            Animator Controller - that plays the open
            animation and opens the doors*/

            anim.SetBool ("Open", true);
        }
    }
    /* This function is called when a Rigidbody exits the trigger
    (stops colliding with our trigger collider).*/
    void OnTriggerExit (Collider other)
    {
        /*Again, we check if the object was the player*/
        if (other.gameObject.CompareTag ("Player")) {
            /*If it's true, we set the bool parameter "Open"
            to false in our Animator Controller. That plays
            the close animation and closes the doors.*/
            anim.SetBool ("Open", false);
        }
    }
}
```

- 35. 将脚本分配给你的 **触发器** 游戏对象，并确保你的角色具有 **刚体** 组件和 `Player` 标签。
- 36. 播放游戏并靠近门；它应该打开。当你离开触发器时，它应该关闭。

## 工作原理...

本教程说明了 Unity 动画工作流程中一个非常重要的特性，即在本地空间中动画化对象的可能性。如果我们动画化子游戏对象，它们的[content]
## 准备工作

要制作动画门，需准备两个对象：**门框** 和 **门**。**门** 应为 **层级** 中 **框架** 的子对象，并置于 *关闭位置*。我们假设打开时 **门** 会向上滑动。还需一个带 `Player` 标签的玩家角色，角色需使用 **刚体** 组件移动或具有运动学 **刚体** 组件。我们使用触发器，触发器仅对刚体起作用。你也可以打开 `Chapter 02 Working with the animation view\Recipe 04 Animating objects local position - creating automatic doors` 目录，找到 `Example.unity` 场景。打开后可在 **层级** 中找到 **自动门** 游戏对象和作为玩家的 **羊** 游戏对象（带 `Player` 标签）。运行游戏靠近门时门会打开。

## 如何操作...

创建自动门按以下步骤操作：

- 1. 选择 **门框** 游戏对象并添加 Animator 组件。在 **检查器** 中点击 **添加组件** 并选择 **Animator**。确保组件加到 **门框** 而非 **门**，即父对象而非子对象。将 **Animator** 组件的 **更新模式** 设为 **动画物理**。
- 2. 选择 **门框** 游戏对象后，打开 **窗口 | 动画** 打开 **动画** 视图。
- 3. 新建动画剪辑命名为 `Closed`，用于关闭门动画。点击回放按钮下下拉列表选择 **创建新剪辑**。
- 4. 点击 **添加属性** 按钮，在列表中找到 **门框** 游戏对象的子对象 **门**。
- 5. 展开 **门** 折叠面板，找到 **变换** 部分，展开后点击 **位置** 属性旁的加号，如下图所示：

![](img/616a75b593bef4212f618360fceea442_71_0.png)

- 6. 会为 **门** 游戏对象的本地位置添加两帧关键帧。保持不动，假设 **门** 在关闭位置。
- 7. 再新建一个动画剪辑命名为门打开。
- 8. 点击 **添加属性**，选择门游戏对象，按步骤5方式添加 **变换 | 位置** 关键帧。
- 9. 将第二关键帧移至约3秒处。
- 10. 选择 **门框** 的子对象 **门**。
- 11. 将其上移至打开位置。确保 **动画** 视图左上角录制按钮激活。播放动画时 **门** 应上移。
- 12. 在 **动画** 视图中选择 **门** 打开动画的最后一帧。打开 **检查器** 复制门的 **变换** 组件，点击组件右上角齿轮选择 **复制组件**。
- 13. 新建动画剪辑命名为 `Opened`。按步骤5为 **门** 子对象添加关键帧。若 **门** 已移动，选择它，点击 **检查器** 中 **变换** 组件齿轮选择 **粘贴组件值**，即可粘贴 `Door Opening` 最后一帧的 **变换** 属性。
- 14. 确保该动画两帧位置相同。
- 15. 再新建最后一个动画剪辑命名为 `Door Closing`。
- 16. 让 **门** 游戏对象从打开位置滑回关闭位置。可复制相应动画帧的 **变换** 组件并粘贴到当前动画。
- 17. 用 **动画** 视图预览所有动画，可用回放按钮下下拉列表切换。
- 18. 满意后关闭 **动画** 视图。选择 **门框** 游戏对象，在 **检查器** 中找到其 Animator 组件。
- 19. 点击 **控制器** 字段，在 **项目** 视图中显示自动生成的 Animator Controller，双击打开。
- 20. 新建一个 `bool` 参数命名为 `Open`。
- 21. 创建状态间的转换循环：**Closed** | **Door Opening** | **Open** | **Door Closing** | **Closed**。见下图：

![](img/616a75b593bef4212f618360fceea442_72_0.png)

- 22. 选择 **Closed** | **Door Opening** 转换，添加条件选择 `Open` 参数设为 true，关闭转换的 **Has Exit Time**。
- 23. 选择 **Opened | Door Closing** 转换，添加条件选择 `Open` 参数设为 false，关闭 **Has Exit Time**。
- 24. **Door Opening | Opened** 和 **Door Closing | Closed** 转换不加条件，确保两者 **Has Exit Time** 启用。
- 25. 关闭 Animator Controller。
- 26. 在场景层级中选择门框游戏对象。
- 27. 为 **门框** 添加 **立方体** 子对象。右键 **门框** 选择 **创建 | 3D 对象 | 立方体**，作为触发器。
- 28. 选择 **立方体** 并缩放使其在门框两侧突出，参考下图：

![](img/616a75b593bef4212f618360fceea442_73_0.png)

- 29. 移除 **立方体** 的 **网格渲染器** 和 **网格过滤器** 组件。点击组件右上角齿轮选择 **移除组件**，仅保留 **变换** 和 **盒状碰撞体**。
- 30. 在 **立方体** 的 **盒状碰撞体** 组件中启用 **Is Trigger**。
- 31. 将立方体游戏对象重命名为 `Trigger`。
- 32. 为 **门框** 游戏对象添加 **网格碰撞体** 组件。
- 33. 为 **门** 游戏对象添加 **盒状碰撞体** 组件或设为 **凸** 的 **网格碰撞体**，并添加 **刚体** 组件，启用 **刚体** 的 **Is Kinematic**。
- 34. 新建 C# 脚本命名为 `AutomaticDoors.cs`，打开后写入以下代码：

```csharp
using UnityEngine;
using System.Collections;

public class AutomaticDoor : MonoBehaviour
{
    /*The anim variable is used to store the reference
    to the Animator component*/

    private Animator anim;

    void Start ()
    {
        /*We assign the Animator component of the parent object
        because this script is attached to the trigger, which
        is the child object of our animated doors*/

        anim = transform.parent.GetComponent<Animator> ();
    }
    /* This function is called when a Rigidbody intersects with
    the collider attached to our game object for the first time.
    Our collider has to be set to trigger. The Collider other
    parameter stores information about the object which collided
    with our trigger (entered the trigger).*/
    void OnTriggerEnter (Collider other)
    {

        //We check the tag of the object entering the trigger
        if (other.gameObject.CompareTag ("Player")) {

            /*If the tag equals "Player", we set the
            bool parameter "Open" to true in our
            Animator Controller - that plays the open
            animation and opens the doors*/

            anim.SetBool ("Open", true);
        }
    }
    /* This function is called when a Rigidbody exits the trigger
    (stops colliding with our trigger collider).*/
    void OnTriggerExit (Collider other)
    {
        /*Again, we check if the object was the player*/
        if (other.gameObject.CompareTag ("Player")) {
            /*If it's true, we set the bool parameter "Open"
            to false in our Animator Controller. That plays
            the close animation and closes the doors.*/
            anim.SetBool ("Open", false);
        }
    }
}
```

- 35. 将脚本分配给 **触发器** 游戏对象，确保角色有 **刚体** 组件和 `Player` 标签。
- 36. 播放游戏靠近门，门应打开；离开触发器时门应关闭。

## 工作原理...

本教程展示了 Unity 动画工作流程的重要特性：可在本地空间动画化对象。动画化子游戏对象时，其位置、旋转和缩放相对于父对象变化。这样可创建动画游戏对象并保存为预制件复用。

关键点如下：

- **Door as a child of Door Frame**：有两个对象：可动的动画 **门** 和静止的 **门框**。**门** 在 **层级** 中是 **门框** 的子对象。动画化时 Animator 组件加到 **门框** 父对象上，**门** 子对象相对父对象动画化。
- **Bool parameter "Open"**：该参数由门 Animator Controller 用于在打开和关闭状态间切换。脚本挂在触发器上设置它。玩家进入触发器时将 `Open` 设为 true，Animator Controller 播放 `Door Opening` 动画；玩家离开触发器时将 `Open` 设为 false，Animator Controller 播放 `Door Closing` 动画。
