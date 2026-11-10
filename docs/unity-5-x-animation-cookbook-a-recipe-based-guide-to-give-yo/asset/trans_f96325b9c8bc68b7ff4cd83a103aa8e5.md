
## 如何运作...

本教程使用与 *使用动画视图为公开脚本变量制作动画* 教程中描述的相同概念。这次我们使用公开脚本变量动画来为粒子系统的属性制作动画，这些属性通常无法从 **Animation** 视图中访问。Unity 秉持着制作 *一切* 动画的理念，所以我认为未来所有粒子系统的属性都将可以直接在 **Animation** 视图中进行动画制作。

## 使用音效剪辑的波形为场景中的物体制作动画

在本教程中，我们将使用音效的波形，根据波形片段的平均音量来创建运动。

### 准备工作

为遵循本教程，我们需要一个音效。你可以使用提供的示例 Unity 项目并进入 `Chapter 07 Special effects\Recipe 09 Using waveform of a sound clip to animate objects in the scene` 目录。如果在那里打开 `Example.unity` 场景并运行游戏，你将能够看到几个立方体对正在播放的音频剪辑做出反应。

### 如何操作...

要使用声音波形进行动画，请按照以下步骤操作：

- 1. 将声音导入 Unity。
- 2. 创建一个空的游戏对象并将其命名为 `AudioSource`。
- 3. 为其添加 Audio Source 组件，并将声音设为 **Loop** 和 **Play On Awake**。将你的声音拖放到 **AudioClip** 字段中。
- 4. 创建一个 **Cube** 游戏对象（前往 **Game Object | 3D Object | Cube**）。
- 5. 创建一个新脚本并命名为 `ScaleWithWaveForm.cs`。在此脚本中，我们有一个 `public AudioSource audioSource` 变量，一个 `public float lerpSpeed` 变量，一个 `float[] samples` 数组，以及一个 `float currentSample` 变量用于存储 `samples` 的平均音量。在此脚本的 `Start()` 函数中，我们将 `samples` 数组的大小设置为 1024（它需要是 2 的幂）。这是我们将从 Audio Source 读取的样本数量。我们还启动一个 IEnumerator SampleAudio() 协程：

```csharp
public AudioSource audioSource;
public float lerpSpeed = 100f;
float[] samples;
float currentSample;

void Start()
{
    samples = new float[1024];
    StartCoroutine("SampleAudio");
}
```

- 6. 此协程使用 audioSource.GetOutputData() 函数获取并将输出音频样本存储在 samples 数组中。然后我们对所有平方样本求和并计算平均音量（RMS）。最后，我们使用该值来缩放 Cube 游戏对象。此外，我们在时间上对缩放变化进行插值以使其更平滑。currentSample 值乘以 10 以使变化更明显（RMS 范围为 0 到 1）：

```csharp
IEnumerator SampleAudio()
{
    while (true)
    {
        audioSource.GetOutputData(samples, 0);
        currentSample = 0f;
        for (int i = 0; i < samples.Length; i++)
        {
            currentSample += (samples[i]*
            samples[i]);
        }
        currentSample = Mathf.Sqrt(currentSample /
        samples.Length);
        transform.localScale =
        Vector3.Lerp(transform.localScale,
        Vector3.one * currentSample*100f,
        Time.deltaTime*4f);
        yield return null;
    }
}
```

- 7. 将脚本分配给 Cube 游戏对象。
- 8. 将 AudioSource 游戏对象拖放到脚本的 Audio Source 字段中。
- 9. 运行游戏以查看效果。

### 如何运作...

本教程的关键组件是 AudioSource.GetOutputData(float[] samples, int channel) 函数。它从当前播放的音频中抓取一定数量的样本。样本数量由 float[] samples 数组的长度指定。此长度必须是 2 的幂。我们在 IEnumerator SampleAudio() 协程中每帧获取样本。我们对平方样本的值取平均以计算音频的 RMS（一定数量样本的平均音量）。平均的样本越多，RMS 的变化越平滑。对于大多数声音，1024 是一个不错的数字。我们使用计算出的 RMS 来缩放 Cube 游戏对象。

### 另请参阅

你可以对波形执行更复杂的信号分析以创建更有趣的效果。例如，如果你想创建一个显示声音不同频率的均衡器，你需要使用快速傅里叶变换。你可以在 Unity 论坛中找到大量关于它的有趣信息。

## 使用动画视图创建昼夜循环

在本教程中，我们将使用动画视图创建一个简单的昼夜循环动画。

### 准备工作

我们将从头开始在 Unity 中创建本教程，因此不需要任何额外资源。你也可以使用提供的示例 Unity 项目并进入 Chapter 07 Special effects\Recipe 10 Creating a day and night cycle with the animation window 目录。那里有一个 Example.unity 场景。

打开它并运行游戏以查看效果。

![](img/616a75b593bef4212f618360fceea442_242_0.png)

简单的昼夜循环

### 如何操作...

要使用动画视图创建昼夜循环，请按照以下步骤操作：

- 1. 创建一个空的游戏对象并命名为 `DayAndNight`。
- 2. 创建另一个空的游戏对象，命名为 `SunAndMoonPivot`，并将其设为 **DayAndNight** 游戏对象的子对象。
- 3. 再创建两个游戏对象，**SunPivot** 和 **MoonPivot**，并将其设为 **SunAndMoonPivot** 的子对象。
- 4. 然后创建 **Sun** 和 **Moon** 对象，并将第一个设为 **SunPivot** 的子对象，第二个设为 **MoonPivot** 的子对象。
- 5. 最后，创建两个 **Directional Lights** 和两个 **Spheres**，并将其中一个分别设为 **Sun** 对象的子对象，剩下的两个设为 **Moon** 对象的子对象。
- 6. 层级结构应如下所示：
  - 1. **DayAndNight** | **SunAndMoonPivot**
  - 2. **SunAndMoonPivot** | **SunPivot**
  - 3. **SunAndMoonPivot** | **MoonPivot**
  - 4. **SunPivot** | **Sun**
  - 5. **MoonPivot** | **Moon**
  - 6. **Sun** | **Directional Light**
  - 7. **Sun | SunSphere**
  - 8. **Moon | Directional Light**
  - 9. **Moon | MoonSphere**
- 7. 创建两个材质：**Moon** 和 **Sun**。为它们选择 **Unlit | Color** 着色器并调整颜色以符合你的喜好（你也可以使用带有自发光颜色的 Standard Shader）。
- 8. 将材质应用到 **SunSphere** 和 **MoonSphere** 对象上。
- 9. 将 **Sun** 和 **Moon** 移离中心。你也可以使用 **SunPivot** 和 **MoonPivot** 来旋转 **Sun** 和 **Moon**。请参考以下截图：

![](img/616a75b593bef4212f618360fceea442_243_0.png)

太阳和月亮的放置

- 10. 选择 **DayAndNight** 游戏对象并为其添加 **Animator** 组件。
- 11. 创建一个新脚本并命名为 `CameraColor.cs`。此脚本有一个 public `Color cameraColor` 变量。在此脚本的 `Update()` 函数中，我们将主摄像机的 `backgroundColor` 设置为 `cameraColor` 变量的值：

```
void Update () {
    Camera.main.backgroundColor = cameraColor;
}
```

- 12. 将脚本分配给 **DayAndNight** 游戏对象。
- 13. 在选择 **DayAndNight** 游戏对象后，打开 **Animation** 视图。
- 14. 创建 **Sun** 和 **Moon** 在地平线上移动的动画。旋转 **SunAndMoonPivot** 以简化操作。
- 15. 为 **Directional Light** 的颜色及其 **Intensity** 和 **Shadow Strength** 制作动画。白天，只有 **Sun's Directional Light** 应投射阴影，夜晚，只有 **Moon's Directional Light** 应投射阴影。
- 16. 如果你使用摄像机的背景颜色作为场景的背景颜色，请使用附加到 **SunAndMoonPivot** 游戏对象的脚本中的 `public Color cameraColor` 为其制作动画。如果使用 Unity 的参数化天空盒，请确保在 **Lighting** 设置中将你的 **Sun's Directional Light** 选择为 **Sun**。这样天空盒会根据 **Sun's Directional Light** 的旋转而变化。
- 17. 运行游戏以查看效果。

### 如何运作...

在本教程中，我们使用 **Hierarchy** 为 **Sun** 和 **Moon** 的旋转制作动画。我们还为附加到这些游戏对象的 **Directional Lights** 的 **Intensity**、**Color** 和 **Shadow Strength** 制作动画。这样，我们可以创造出昼夜循环的错觉。

### 还有更多...

要控制昼夜循环动画的速度，你可以在运行时从脚本中修改 `anim.speed` 参数。`anim` 变量是对附加到 **DayAndNight** 游戏对象的 **Animator** 组件的引用。
