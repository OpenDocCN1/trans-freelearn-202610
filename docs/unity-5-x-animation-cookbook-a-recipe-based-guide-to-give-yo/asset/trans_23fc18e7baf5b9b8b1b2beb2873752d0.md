
## 从 3D 软件导入完整的过场动画

在 Unity 中创建过场动画的最佳方式是从 3D 软件导入完整的动画。我们在之前的食谱中做过类似的事情（在导入到 Unity 之前在 3D 软件中同步动画）。在这里我们将创建一个带有摄像机动画的完整过场动画。

### 准备工作

对于本食谱，我们需要与上一食谱相同的角色，并带有 *kicking the ball* 动画。此外，我们在动画中添加了三个简单的立方体。这些立方体是动画化的，表示摄像机。我们使用立方体而不是空对象或摄像机，是因为 Unity 在导入既不是骨架也不是具有网格渲染器的对象时有时会出现问题：

![](img/616a75b593bef4212f618360fceea442_253_0.png)

Blender 中的过场动画对象。红色立方体保存摄像机位置

你也可以前往 `Chapter 08 Animating Cutscenes\Recipe 04 Importing a whole cutscene from a 3D package` 目录。你可以在那里找到 `Example.unity` 场景。如果运行游戏，你会看到一个角色踢足球。摄像机会随动画进程进行动画并切换。你可以在 `Animations` 目录中找到所有需要的动画。

### 操作方法

要从 3D 软件导入完整的过场动画，请按照以下步骤操作：

- 1. 在 3D 软件中创建过场动画。记住所有对象的动画长度应相同。它们应在第一帧和最后一帧上都有关键帧。
- 2. 如果计划使用 **Humanoid** 骨架，请在导入 Unity 之前将每个角色保存为单独的文件。
- 3. 将过场动画导入 Unity（如果需要，还包括所有角色）。使用 **Generic** 骨架的所有对象可以一起导出为一个文件。在这种情况下，会为整个过场动画创建一个 **Generic** 骨架。
- 4. 如果你使用 Blender 并希望将过场动画导出为 FBX 格式，请使用 6.1 版本并确保勾选 **Default Take** 选项。
- 5. 在 **Import Settings** 中进入 **Animation** 选项卡。
- 6. **Default Take** 包含整个过场动画。你可能会在 Animation 选项卡中看到额外的片段（尤其是在使用骨骼动画时）。不要使用它们。你可以从导入设置中删除它们，并将 **Default Take** 片段名称更改为更具描述性的名称。
- 7. 如果角色使用 **Humanoid** 骨架，请从单独的文件中逐一导入它们。
- 8. 为过场动画创建一个 **Animator Controller**，并为每个角色创建一个（如果使用 **Humanoid** 骨架）。
- 9. 将过场动画放置在控制器中。
- 10. 将所有对象放置在场景的同一位置。
- 11. 如果你想为摄像机做动画，为过场动画的每个镜头创建一个摄像机。
- 12. 将摄像机设为过场动画中动画化的摄像机持有对象的子对象。你可能需要调整摄像机旋转。
- 13. 我们的摄像机是摄像机持有对象（以及过场动画对象）的子对象。但动画是导入的且只读。因此，我们无法像 *Changing cameras with animation* 食谱中那样更改摄像机。我们需要在过场动画的根对象（带有 Animator 组件的对象）上添加一个脚本。创建一个新脚本并命名为 `CutsceneCameraChanger.cs`。在此脚本中，我们有一个所有过场动画摄像机的列表，以及一个 `public void ChangeCamera()` 动画事件，该事件根据给定的 `int newCameraIndex` 参数更改摄像机。它会禁用列表中的其他所有摄像机：
```
public void ChangeCamera (int newCameraIndex)
{
for (int i = 0; i < cutsceneCameras.Length; i++)
{
    cutsceneCameras[i].gameObject.
        SetActive(false);
}
    cutsceneCameras[newCameraIndex].gameObject.
        SetActive(true);
}
```
- 14. 将脚本分配给过场动画的根对象。
- 15. 打开过场动画的 **Animation Import Settings**。
- 16. 将时间线调整到你想要切换镜头的位置。
- 17. 添加 Animation Events 以启用适当的摄像机并切换镜头。使用 `int` 参数为 `ChangeCamera` 函数提供 `newCameraIndex`。记住将摄像机的 **Depth** 参数设置为高于游戏摄像机。
- 18. 应用 **Animation Import Settings** 并禁用每个摄像机持有对象的 **Mesh Renderer** 组件，以便它们在游戏中不可见。
- 19. 运行游戏以查看效果。

### 工作原理

Unity 可以将整个过场动画作为单个资源导入。你可以用大量对象和角色制作过场动画，然后导出为 FBX（如果你使用 Blender，也可以直接保存文件）。导入后，**Default Take** 包含整个动画。你无法更改动画（它是只读的），但可以使用脚本通过 Animation Events 启用/禁用摄像机。不过你需要在 Animation Import Settings 中添加事件。

如果你计划为不同角色重用动画，需要将过场动画中的每个骨架作为单独文件导出。只有这样才能将其设置为 **Humanoid**。

你无法从 3D 软件导出粒子系统。仅支持变换和骨骼动画。你需要在 Unity 中创建 VFX 并使用 Animation Events 与过场动画同步。音效也适用相同规则。我们在第 7 章《Special Effects》的 *Using Animation Events to trigger sound and visual effects* 食谱中做过此事。

### 还有更多

根据 Unity 的路线图，Director 工具计划在 5.x 时间表中的某个时候推出。它可能会使在 Unity 中制作过场动画变得更容易。但对于习惯使用自己喜欢的 3D 软件的动画师来说，导入完整的过场动画仍然可能是首选方案。

## 同步字幕

在本食谱中，我们创建字幕并将其与动画同步：

![](img/616a75b593bef4212f618360fceea442_256_0.png)

与动画同步的字幕

### 准备工作

在本食谱中，我们将使用与上一食谱相同的过场动画。你也可以打开示例 Unity 项目并前往 `Chapter 08 Animating Cutscenes\Recipe 05 Synchronizing subtitles` 目录。如果在那里打开 `Example.unity` 场景并运行游戏，你会看到随动画同步变化的字幕。

### 操作方法

要将字幕与过场动画同步，请按照以下步骤操作：

- 1. 按照上一食谱中的方式导入过场动画。
- 2. 创建一个新的 Image（前往 Game Object | UI | Image）。
- 3. 将其命名为 SubtitlesContainer。
- 4. 创建一个新的 Text 游戏对象并将其设为 SubtitlesContainer 的子对象。为此，右键单击 SubtitlesContainer 并选择 UI | Text。
- 5. 将 SubtitlesContainer 的 Image 组件颜色更改为半透明黑色。将 Text 组件的颜色更改为白色。按需调整对象的位置和大小。
- 6. 将模型放置在场景中。
- 7. 创建一个新的脚本并命名为 Subtitles.cs。此脚本有一个 string subtitlesText 变量，我们将在其中存储字幕中显示的文本。它还具有对 SubtitlesContainer 和 Text 游戏对象的公共引用：public GameObject subtitlesContainer 和 public Text textComponent。此脚本有一个 public void SetSubtitles(string text) 函数。它从 Animation Event 调用并用于设置 subtitlesText 变量的值：
```
public void SetSubtitles(string text)
{
    subtitlesText = text;
}
```
- 8. 在此脚本的 Update() 函数中，我们检查 subtitlesText 变量是否包含任何文本。如果有，我们启用 SubtitlesContainer 游戏对象并设置 Text 游戏对象以显示我们的字幕。如果 subtitlesText 为空字符串，我们禁用 SubtitlesContainer：
```
if (string.IsNullOrEmpty(subtitlesText)
&& subtitlesContainer.activeSelf)
{
    subtitlesContainer.SetActive(false);
}
else if (!string.IsNullOrEmpty(subtitlesText)
&& !subtitlesContainer.activeSelf)
{
    subtitlesContainer.SetActive(true);
}
else
{
```
