
```
textComponent.text = subtitlesText;
}
```

- 9. 将脚本附加到过场动画游戏对象（拥有 Animator 组件并为所有道具做动画的对象）上。
- 10. 打开过场动画的动画导入设置。
- 11. 你可以在时间线上拖动并添加动画事件。
- 12. 添加一个动画事件并将函数命名为 SetSubtitles。在 String 参数中输入你想要显示为字幕的文本，如以下截图所示：
- 13. 如果你想让字幕消失，添加一个 SetSubtitles 动画事件并将 String 参数留空。
- 14. 应用动画导入设置。
- 15. 运行游戏查看效果。

![](img/616a75b593bef4212f618360fceea442_258_0.png)

SetSubtitles 动画事件，文本为 "Gooaal!"

## 工作原理...

在本教程中，我们使用动画事件将字幕与过场动画同步。其原理与第 7 章《特殊效果》中《使用动画事件触发音效和视觉效果》教程相同。`Subtitles.cs` 脚本还根据通过动画事件调用的 `public void SetSubtitles(string text)` 函数所设置的 `subtitlesText` 变量的值来处理字幕的显示和隐藏。

如果你想本地化字幕，请使用字符串本地化键而不是文本。然后 `Subtitles.cs` 脚本需要使用这些键在你的本地化库中查找文本。

## 使用根运动在游戏过程中播放过场动画

我们也可以在不改变摄像机的情况下在游戏过程中播放过场动画，并使用平滑过渡进入和退出过场动画。本教程展示了该方法的示例。

### 准备工作

在本教程中，我们将使用与上一教程相同的过场动画。你也可以打开示例 Unity 项目并进入 `Chapter 08 Animating Cutscenes\Recipe 06 Using root motion to play cutscenes during gameplay` 目录。如果在那里打开 `Example.unity` 场景并运行游戏，你将能够移动角色。靠近红色球体标记并按下空格键即可播放过场动画：

![](img/616a75b593bef4212f618360fceea442_259_0.png)

游戏过程中播放的过场动画

### 操作方法...

要在游戏过程中播放过场动画，请按以下步骤操作：

- 1. 导入你的过场动画。确保将角色动画作为单独文件导入。
- 2. 进入过场动画资源导入设置中的动画选项卡。
- 3. 将 Default Take 动画重命名为 Cutscene。
- 4. 创建一个新的动画剪辑，选择 Default Take 作为源，并将起始帧设为 0、结束帧设为 1。将此动画命名为 CutsceneIdle 并使其循环播放。
- 5. 将过场动画游戏对象（在我们的示例中是 Ball 游戏对象）和角色放置在场景中。
- 6. 按照第 4 章《角色移动》中的《使用根运动控制角色》教程操作。这样你将拥有一个可控制的移动角色。
- 7. 为过场动画创建一个 Animator Controller。
- 8. 将 CutsceneIdle 设为默认动画。
- 9. 拖放 Cutscene 动画。
- 10. 创建一个 Trigger 参数并将其命名为 Cutscene。
- 11. 使用 Cutscene Trigger 参数从 CutsceneIdle 到 Cutscene 动画创建过渡。将 Has Exit Time 设为 false，Transition Duration 设为 0.1 秒。
- 12. 将 Player 标签分配给角色。
- 13. 将过场动画添加到角色的 Animator Controller 中。在我们的示例中，动画名为 SoccerKick。
- 14. 在角色的控制器中添加一个 Cutscene Trigger 参数。
- 15. 使用 Cutscene Trigger 作为条件从 Any State 创建到 SoccerKick 的过渡。Has Exit Time 应设为 false，Transition Duration 设为约 0.5 秒。
- 16. 再添加一个从 SoccerKick 到 Idle 的过渡，无条件，Has Exit Time 设为 true。Transition Duration 也应设为约 0.5 秒。
- 17. 创建一个新的 C# 脚本并命名为 CutsceneTrigger.cs。在此脚本中，我们有 void OnTriggerEnter() 和 void OnTriggerExit() 函数。它们的主要任务是设置 bool inTrigger 标志。OnTriggerEnter() 函数还存储对玩家游戏对象的引用：
```
void OnTriggerEnter (Collider other) {
    if (other.gameObject.CompareTag("Player"))
    {
        player = other.gameObject;
        inTrigger = true;
    }
}
void OnTriggerExit(Collider other)
{
    if (other.gameObject.CompareTag("Player"))
    {
        inTrigger = false;
    }
}
```
- 18. 我们在 Update() 函数中检查 inTrigger 标志的值。如果其设为 true，玩家可以按空格键开始过场动画。这会启动一个协程，使角色的位置和旋转与触发器的位置和旋转匹配。此位置和旋转是角色在过场动画开始时应具有的位置和旋转。我们还会启用或禁用提示，其引用存储在 public GameObject onScreenInfo 变量中。当玩家开始过场动画时，我们关闭 Box Collider 组件以禁用触发器：
```
if (inTrigger)
{
    onScreenInfo.SetActive(true);

    if (Input.GetKeyDown(KeyCode.Space))
    {
        GetComponent<BoxCollider>().enabled = false;
        inTrigger = false;
        StartCoroutine("StartCutscene");
    }
}
else
{
    onScreenInfo.SetActive(false);
}
```
- 19. 在 IEnumerator StartCutscene() 协程中，我们首先将角色的 Rigidbody 组件设为 Kinematic。这样我们就关闭了物理模拟。然后我们插值玩家的位置和旋转，使其与触发器的位置和旋转匹配。如果足够接近，我们就在玩家和过场动画的 Animator 组件中设置 Cutscene Trigger，以便在两个对象上同时播放动画：
```
IEnumerator StartCutscene()
{
    player.GetComponent<Rigidbody>().isKinematic = true;
    bool positionAdjusted = false;
    bool rotationAdjusted = false;
    while(true)
    {
        yield return null;
        if ((player.transform.position -
        transform.position).magnitude <= 0.01f)
        {
            positionAdjusted = true;
        }
        else
        {
            player.transform.position =
            Vector3.Lerp(player.transform.position,
            transform.position, positionAdjustmentSpeed
            * Time.deltaTime);
        }
        if (Vector3.Angle(player.transform.forward,
        transform.forward) <= 1f)
        {
            rotationAdjusted = true;
        }
        else
        {
            player.transform.rotation =
            Quaternion.Lerp(player.transform.rotation,
            transform.rotation, positionAdjustmentSpeed
            * Time.deltaTime);
        }
        if (positionAdjusted && rotationAdjusted)
        {
            break;
        }
    }
    player.GetComponent<Animator>
    ().SetTrigger(animationTrigger);
    cutsceneAnimator.SetTrigger(animationTrigger);
}
```
- 20. 创建一个新的空游戏对象并将其命名为 `CutsceneTrigger`。为其添加 **盒状碰撞体** 组件并将其设为 **是否触发**。
- 21. 将其放置在角色在过场动画中必须所在的确切位置。将其旋转方式与角色所需旋转方式相同。你可以使用过场动画中的空对象（从 3D 软件导出）来简化操作。
- 22. 将 `CutsceneTrigger.cs` 脚本分配给 **CutsceneTrigger** 游戏对象。
- 23. 创建一个提示 UI 文本并将其分配给 **Cutscene Trigger** 组件的 **On Screen Info** 字段。
- 24. 将过场动画游戏对象的 **Animator** 组件分配给 **Cutscene Trigger** 组件的 **CutsceneAnimator** 字段。
- 25. 再创建一个 C# 脚本并命名为 `SetKinematic.cs`。在此脚本中，我们只有一个 `public void NotKinematic()` 函数，其中我们将 **Rigidbody** 组件设为非运动学：
- 26. 将脚本附加到角色。
- 27. 在选中角色的情况下，打开 **Animation** 视图。
- 28. 选择 **SoccerKick** 动画。
- 29. 在动画接近结尾处添加一个 **Animation Event** 并选择 **NotKinematic()**。这将使角色再次对物理做出反应。
- 30. 运行游戏，靠近触发器并按空格键查看效果。
```
public void NotKinematic()
{
    GetComponent<Rigidbody>().isKinematic = false;
}
```

### 工作原理...

本教程使用与 `Chapter 5, Characters Actions and Expressions` 中 *行动点 – 在指定位置执行动作* 教程相同的概念。这里我们为动画道具（在我们的示例中是 Ball 游戏对象）添加了一个一帧循环的 CutsceneIdle 动画。这使得对象等待过场动画开始。我们的角色过场动画以单独文件导出，这使我们能够将其骨架设为 **Humanoid** 并与其他动画一起使用。

我们的角色与过场动画游戏对象在同一时刻开始播放动画。这使它们同步。

另一件重要的事情是，我们在过场动画期间将角色的 **Rigidbody** 组件设为 **Is Kinematic**。这使得动画以 100% 的权重控制我们的角色。没有碰撞可以打断角色播放动画。过场动画结束后，我们关闭 **Is Kinematic** 选项以使角色恢复正常行为。
