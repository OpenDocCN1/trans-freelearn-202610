
最后一步是前往 **Drawing** 属性并勾选使用 **Local Coords** 的复选框。这只是确保粒子遵循自身的路径，而不是它们可能附加到的任何父节点的路径。它使粒子保持在我们在场景中放置的位置。如图 10.19 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_291_0.png)

*图 10.19 – 在 Drawing 属性中设置粒子使用局部坐标*

要预览此彩纸粒子效果，将 **Inspector** 滚动到最顶部并打开 **Emitting** 复选框。每次勾选时，你应该能在场景中看到蓝色彩纸粒子发射，如图 10.20 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_291_1.png)

*图 10.20 – 打开 Emitting 会发射粒子*

我们现在有一个天蓝色彩纸粒子的场景，但彩纸炮应该向天空发射彩虹色的粒子。

要实现这一点，请复制蓝色粒子，然后右键单击 ParticleProcessMaterial 并选择 Make Unique，然后在 Display 属性下更改颜色（我们已在图 10.17 中这样做）并重命名粒子节点以匹配。如图 10.21 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_292_0.png)

图 10.21 – 彩纸炮场景树

请注意，并非所有彩纸粒子都在发射。我们将在下一节中修复这个问题。

## 脚本化彩纸炮

为了确保所有彩纸粒子在正确的时间发射，我们将脚本附加到 Confetti_Cannon 节点并按如下方式定义 fire() 函数。

```
extends Node2D

func fire():
    $Blue.emitting = true
    $Orange.emitting = true
    $Green.emitting = true
    $Yellow.emitting = true
    $Red.emitting = true
    $Pink.emitting = true
```

此 fire() 函数通过将它们的 emitting 属性设置为 true 一次性打开所有彩纸粒子发射器。每一行都针对不同的颜色粒子节点——Blue、Orange、Green、Yellow、Red 和 Pink——并同时激活它们。当调用 fire() 函数时，这会产生庆祝性的彩色彩纸爆发效果。

通过将所有发射器保留在 Confetti_Cannon 节点内并通过此一个函数触发它们，我们保持代码井然有序，并使使用单个命令发射所有彩纸变得容易。

请回忆，炮在玩家完成关卡并触碰检查点旗帜时发射彩纸作为小小的视觉奖励。因此，将炮附加到检查点是有意义的。我们可以在 Godot 中通过将 confetti_cannon.tscn 文件拖入 CheckPoint 场景的场景树中轻松实现这一点，如图 10.22 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_293_0.png)

图 10.22 – 将彩纸炮添加到 CheckPoint 场景

一旦彩纸炮成为 CheckPoint 场景的一部分，我们需要在 check_point.gd 中添加一些代码，以便在关卡完成后调用 fire 方法。

在场景树中单击 **CheckPoint (Area2D)** 节点以选中它。现在，单击 **Node** 选项卡，然后单击 **Signals** 选项卡。连接 on_body_entered() 信号并按如下方式编写代码：

```gdscript
func _on_body_entered(body):
    if level_complete:
        $Confetti_Cannon.fire()
```

前面的代码在接收到 _on_body_entered() 信号时被调用。这意味着玩家已与检查点旗帜发生碰撞。然后，如果 level_complete 布尔值设置为 true，则会调用彩纸炮的 fire() 方法。fire() 方法随后将炮中所有彩纸的 emitting 属性设置为 true。

现在一切就绪，当你完成关卡并触碰检查点旗帜时，你将获得一次令人愉悦的彩纸爆发作为奖励。

在最后一节中，我们将学习如何为关卡添加音效和音频，以增加另一层精致感和氛围。

## 添加音频和音效

声音是游戏开发中的强大工具，以视觉 alone 无法实现的方式塑造玩家的体验。精心制作的原声带设定了氛围，无论是快节奏的动作序列还是平静的探索场景。[content]
最后一步是前往 **Drawing** 属性并勾选使用 **Local Coords** 的复选框。这只是确保粒子遵循自身的路径，而不是它们可能附加到的任何父节点的路径。它使粒子保持在我们在场景中放置的位置。如图 10.19 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_291_0.png)

*图 10.19 – 在 Drawing 属性中设置粒子使用局部坐标*

要预览此彩纸粒子效果，将 **Inspector** 滚动到最顶部并打开 **Emitting** 复选框。每次勾选时，你应该能在场景中看到蓝色彩纸粒子发射，如图 10.20 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_291_1.png)

*图 10.20 – 打开 Emitting 会发射粒子*

我们现在有一个天蓝色彩纸粒子的场景，但彩纸炮应该向天空发射彩虹色的粒子。

要实现这一点，请复制蓝色粒子，然后右键单击 ParticleProcessMaterial 并选择 Make Unique，然后在 Display 属性下更改颜色（我们已在图 10.17 中这样做）并重命名粒子节点以匹配。如图 10.21 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_292_0.png)

图 10.21 – 彩纸炮场景树

请注意，并非所有彩纸粒子都在发射。我们将在下一节中修复这个问题。

## 脚本化彩纸炮

为了确保所有彩纸粒子在正确的时间发射，我们将脚本附加到 Confetti_Cannon 节点并按如下方式定义 fire() 函数。

```
extends Node2D

func fire():
    $Blue.emitting = true
    $Orange.emitting = true
    $Green.emitting = true
    $Yellow.emitting = true
    $Red.emitting = true
    $Pink.emitting = true
```

此 fire() 函数通过将它们的 emitting 属性设置为 true 一次性打开所有彩纸粒子发射器。每一行都针对不同的颜色粒子节点——Blue、Orange、Green、Yellow、Red 和 Pink——并同时激活它们。当调用 fire() 函数时，这会产生庆祝性的彩色彩纸爆发效果。

通过将所有发射器保留在 Confetti_Cannon 节点内并通过此一个函数触发它们，我们保持代码井然有序，并使使用单个命令发射所有彩纸变得容易。

请回忆，炮在玩家完成关卡并触碰检查点旗帜时发射彩纸作为小小的视觉奖励。因此，将炮附加到检查点是有意义的。我们可以在 Godot 中通过将 confetti_cannon.tscn 文件拖入 CheckPoint 场景的场景树中轻松实现这一点，如图 10.22 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_293_0.png)

图 10.22 – 将彩纸炮添加到 CheckPoint 场景

一旦彩纸炮成为 CheckPoint 场景的一部分，我们需要在 check_point.gd 中添加一些代码，以便在关卡完成后调用 fire 方法。

在场景树中单击 **CheckPoint (Area2D)** 节点以选中它。现在，单击 **Node** 选项卡，然后单击 **Signals** 选项卡。连接 on_body_entered() 信号并按如下方式编写代码：

```gdscript
func _on_body_entered(body):
    if level_complete:
        $Confetti_Cannon.fire()
```

前面的代码在接收到 _on_body_entered() 信号时被调用。这意味着玩家已与检查点旗帜发生碰撞。然后，如果 level_complete 布尔值设置为 true，则会调用彩纸炮的 fire() 方法。fire() 方法随后将炮中所有彩纸的 emitting 属性设置为 true。

现在一切就绪，当你完成关卡并触碰检查点旗帜时，你将获得一次令人愉悦的彩纸爆发作为奖励。

在最后一节中，我们将学习如何为关卡添加音效和音频，以增加另一层精致感和氛围。

## 添加音频和音效

声音是游戏开发中的强大工具，以视觉 alone 无法实现的方式塑造玩家的体验。精心制作的原声带设定了氛围，无论是快节奏的动作序列还是平静的探索场景。同时，音效为玩家操作提供即时反馈，使游戏世界感觉更生动、更具响应性。

在本节中，我们将探讨如何通过音频增强游戏的感觉。我们将添加背景音乐以营造氛围，并为关键交互引入音效，例如收集物品、受到伤害或完成关卡。到本节结束时，你将了解如何实现和微调音频以使游戏更具沉浸感和吸引力。

那么，让我们通过添加一些有趣的背景音乐使关卡更愉快。**Incompetech** 是一个很好的免版税音乐资源：

https://incompetech.com/music/royalty-free/music.html

我从该网站选择了 Kevin MacLeod 的曲目 *Adventures in Adventureland*。该曲目采用知识共享：署名 4.0 许可（http://creativecommons.org/licenses/by/4.0/）。

让我们开始在 Assets 文件夹中新建一个名为 Audio 的子文件夹，并将 Adventures in Adventureland.mp3 文件拖放到其中，如图 10.23 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_295_0.png)

图 10.23 – 将背景音乐添加到我们的音频文件夹

要播放音频，Godot 有一个名为 AudioStreamPlayer 的特殊节点。它非常适合播放背景音乐，因为它不会以位置方式播放音频。如果你希望音频从屏幕上的不同来源播放，则使用 AudioStreamPlayer2D 或 AudioStreamPlayer3D 节点。

由于关卡将有背景音乐，请向关场景添加一个新的 AudioStreamPlayer 节点，如图 10.24 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_295_1.png)

图 10.24 – 向关卡场景添加 AudioStreamPlayer 节点

选中 AudioStreamPlayer 后，查看 Inspector 中的属性。这里，Stream 属性是它将播放的音频源。将 MP3 文件拖放到 Stream 中。将音量降低到 −20 dB 并将 Autoplay 设置为 On。所有这些更改都反映在图 10.25 中：

![](img/d34207e07c133a9e84d9caf1d479006c_296_0.png)

图 10.25 - 设置我们的音频播放器的流

因为 **Autoplay** 设置为 true，曲目将在关卡加载后立即开始播放。如果你想停止音乐，所有这些设置都可以通过附加到关卡的脚本更改，并且可以将播放设置为 false。

现在我们已经让背景音乐运行起来，让我们继续为游戏添加音效。

### 实现音效

音效不会自动且持续播放。相反，它们由事件触发或与事件配对。一个很好的例子是玩家跳跃或收集东西时。

首先，你需要下载你选择的音效。你可以访问 **Open Game Art**（https://opengameart.org/）并搜索跳跃音效和硬币或可收集物音效。该网站是免费游戏资产的绝佳资源，所有内容均在知识共享 CC0 许可下提供。

一旦你有合适的跳跃音效和收集音效，请将文件添加到 Godot **FileSystem** 中的 Audio 文件夹中。这在图 10.26 中显示：

![](img/d34207e07c133a9e84d9caf1d479006c_296_1.png)

图 10.26 - 向我们的 Audio 文件夹添加音效

由于我们的游戏中很简单，我们只有两个音效，一个用于 *jumping*，一个用于 *collecting*，我们将在 **Player** 场景中添加两个 **AudioStreamPlayer** 节点来处理此事。重命名 **AudioStreamPlayer** 节点以反映它们将播放的音效，如 *图 10.27* 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_297_0.png)

图 10.27 – 向 Player 场景添加多个音频流

对于具有多个重叠音效的更复杂游戏，添加多个 **AudioStreamPlayer** 节点不是最佳解决方案，你会考虑 **Sound Manager** 场景或为音乐和音效设置一些音频总线。

每个 **AudioStreamPlayer** 节点都应播放关联的音效，因此请确保将正确的文件拖到每个 **Stream**，如 *图 10.28* 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_297_1.png)

图 10.28 – 两个不同音效的两个音频流
