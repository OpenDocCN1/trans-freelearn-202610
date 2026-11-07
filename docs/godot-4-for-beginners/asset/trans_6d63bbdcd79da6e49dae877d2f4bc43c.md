
4. 将 cloud_1.obj 文件拖入 Mesh 属性中。这在图 9.14 中显示：

![](img/d34207e07c133a9e84d9caf1d479006c_263_0.png)

图 9.14 – 使用其中一个云作为 Mesh 来表示烟雾

5. 记得通过应用 palette.png 纹理来为网格贴图。

6. 展开 Gravity 部分并使 y 值变为正值。完成后，烟雾会上升。

7. 将 Amount 属性减少到 1，以便一缕烟雾从大炮中升起。

8. 将 One Shot 属性设置为 On。这在图 9.15 中显示：

![](img/d34207e07c133a9e84d9caf1d479006c_263_1.png)

图 9.15 - 更改数量并将 One Shot 设置为 On

9. 通过展开 **Node3D** 的 **Transform** 部分并将 z 轴旋转 90 度，使云垂直定向。这在图 9.16 中显示：

![](img/d34207e07c133a9e84d9caf1d479006c_264_0.png)

图 9.16 – 在 z 轴上旋转模型使其垂直

10. 随时间调整云的缩放比例，从 0 到完整大小（1）。通过展开 **Scale** 部分并将 **Scale Amount Minimum** 设置为 0，**Scale Amount Maximum** 设置为 1 来实现。

11. 通过点击 **New Curve** 创建 **Scale Amount** 曲线并随时间调整缩放比例，如图 9.17 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_264_1.png)

图 9.17 – 设置最小值、最大值和缩放曲线

这将缩放比例从 0 设置到 1；不过，我建议尝试不同的曲线值，直到找到满意的结果。

烟雾场景已完成，可以在 Cannon 脚本中实例化。当我们在 Cannon 脚本中实例化一个炮弹时，之后应立即实例化烟雾。现在，让我们返回到 Cannon 脚本。

12. 添加两个新的全局变量 particles（用于引用粒子场景文件）和 smoke（作为场景的实例）：

```
extends Node3D
var ball_scene
var ball_instance
var marker
var particles
var smoke
```

13. 修改 _ready 函数，以便我们也预加载烟雾粒子场景。以下代码中新增的行已注释。创建烟雾实例，将其定位到标记处，使其成为 Cannon 场景的子节点，并将 emitting 属性设置为 true：

```
func _ready():
    scene = preload("res://scenes/ball.tscn")
    particles = preload("res://scenes/smoke.tscn")
    ball = scene.instantiate()

    # Create an instance of the smoke particles
    smoke = particles.instantiate()

    marker = $Marker3D
    ball.position = marker.position

    # Also place the smoke effect at the Marker3D position
    smoke.position = marker.position

    add_child(ball)

    # Add the smoke effect to the current scene as a child
    add_child(smoke)
```

```gdscript
# Start the smoke particle effect so it emits immediately
smoke.emitting = true
```

14. 最后，我们必须在 timeout 函数中创建新实例，以便每次大炮发射时都出现一缕烟雾：

```gdscript
func _on_cannonball_timer_timeout():
    ball_instance = ball_scene.instantiate()
    smoke = particles.instantiate()
    smoke.position = marker.position
    ball_instance.position = marker.position
    add_child(ball_instance)
    add_child(smoke)
    smoke.emitting = true
```

通过对大炮脚本的这些修改，你可以再次测试关卡。你会注意到每次发射炮弹时，都会有两朵小烟云飘向空中。这是一种为关卡增添动感的简单方法。

另一种使关卡更具吸引力的方法是添加音频，例如音效和背景音乐。在下一节中，我们将学习如何处理音频。

## Adding audio to our level

如果玩家角色在跳跃时发出声音会很有趣！让我们添加一些音频！

- 1. 为此，我们需要自己录制声音或找到一个处于公共领域且具有 Creative Commons 许可的声音。这里有一个可用于本项目的声音：https://opengameart.org/content/jump-sound-16bit.
- 2. 下载 jump.wav 文件。在 **FileSystem** 中，进入 assets 文件夹，并创建一个名为 audio 的子文件夹。按图 9.18 所示将声音文件拖入其中：

![](img/d34207e07c133a9e84d9caf1d479006c_267_0.png)

图 9.18 – 将 jump.wav 文件添加到我们的 audio 文件夹

3. 打开 Player 场景并添加一个 AudioStreamPlayer 节点作为子节点。在 Stream 属性中，拖放音频文件。保持其他属性不变。这在图 9.19 中显示：

![](img/d34207e07c133a9e84d9caf1d479006c_267_1.png)

图 9.19 – 为 AudioStreamPlayer 提供要播放的声音文件

4. 打开 Player 脚本并为 AudioStreamPlayer 添加一个引用变量到你的 onready 变量列表中：

```
@onready var model = $MeshInstance3D
@onready var jump_cooldown_timer = $Jump_cooldown_timer
@onready var audio_stream_player = $AudioStreamPlayer
```

5. 我们只希望在玩家跳跃时播放跳跃音效。为此，我们告诉 AudioStreamPlayer 在 jump 函数运行时播放文件。将该请求添加为 jump 函数的第一行：

```
func jump():
    audio_stream_player.play()
```

6. 这就是在玩家跳跃时播放音效所需做的全部。

但如果我们想在玩家落地时播放音效呢？

1. 从 Open Game Art 获取另一个 CC0 许可的音效（https://opengameart.org/content/jump-landing-sound）并将其添加到 audio 文件夹。

2. 为了在正确的时间播放正确的声音，代码必须调整为先将对应的音频文件加载到 AudioStreamPlayer 中，然后再尝试播放。

3. 在 jump 函数的第一行添加另一行，先加载文件再播放：

```
func jump():
    audio_stream_player.stream =
    preload("res://assets/audio/jump.wav")
    audio_stream_player.play()
```

4. 同样地，当玩家落地时，我们必须加载对应的音频文件然后播放。在 handle_respawn() 函数中，在玩家落地时缩放玩家之后添加这些行即可：

```
if is_on_floor() and gravity > 2 and !previously_floored:
    model.scale = Vector3(1.25, 0.75, 1.25)
    audio_stream_player.stream =
    preload("res://assets/audio/jumpland.wav")
    audio_stream_player.play()
```

5. 使用代码中的技术，我们确保在播放之前将正确的音频文件加载到播放器中。下一步是在关卡中添加背景音乐。首先下载背景音乐（https://opengameart.org/content/background-music-2-the-ice-caves）并将其添加到 audio 文件夹。

6. 这次，我们将在 **Level 1** 场景中添加一个 **AudioStreamPlayer** 节点。我们将 Background Music 2.ogg 文件拖放到 **Stream** 属性中。将 **Autoplay** 属性设置为 **On**。这在图 9.20 中显示：

![](img/d34207e07c133a9e84d9caf1d479006c_269_0.png)

图 9.20 - 设置 Stream 播放器的 Autoplay 属性

7. 还有最后一步，那就是确保音轨循环。展开 **Parameters** 部分并按图 9.21 所示打开循环：

![](img/d34207e07c133a9e84d9caf1d479006c_269_1.png)

图 9.21 - 为背景音乐打开循环

如图 9.21 所示，通过打开循环，我们可以让背景音乐在关卡中重复播放。

在游戏设计中，重复性运动常用于创建动态逼真的效果，例如云朵在天空中漂移、海浪拍打岸边，或道具缓慢旋转或上下悬浮。这类运动通常通过使用正弦波的振荡，以平滑自然的方式来回调整物体的属性（例如位置、大小或角度）来实现。在我们最后的部分中，我们将使用正弦函数使关卡中的云朵缓慢地来回移动。

## Using the sine function

如前所述，与其让云朵静止在天空中，我们将使用正弦波使它们来回漂移。返回 Cloud 场景并将脚本附加到根节点（Cloud）。

使云朵漂移的完整代码如下所示，之后将逐行解释：

```gdscript
extends Node3D
@onready var mesh_instance_3d: MeshInstance3D = $MeshInstance3D
var time: float
func _process(delta):
    time += delta
    mesh_instance_3d.position =
    Vector3(get_sine(),get_sine(),get_sine())

func get_sine():
    return sin(time * 0.5) * 0.5
```

让我们在这里分解这段代码，以便更好地理解：

- 声明一个名为 time 的 float 变量来跟踪经过的时间。它用于计算移动的正弦波。
- 为云的 StaticBody3D 节点创建了一个引用，因为这本质上是我们将要移动的云对象。
- 在 process 函数中，我们每帧增加 time 变量。这确保正弦波动画随时间平滑进展。
- 接下来，在每一帧更新云（StaticBody3D）的 position 属性。
- x,y, 和 z 坐标使用 get_sine() 函数设置，该函数产生平滑的振荡值。
- get_sine() 函数基于 time 变量计算平滑的振荡。
