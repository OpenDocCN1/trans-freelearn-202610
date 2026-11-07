
2. 前述代码中的变量均未初始化，也就是说它们没有初始值。我们将在 _ready() 函数中为它们赋值：

```
# Called when the node is added to the scene tree
func _ready():
    # Load (preload) the Ball scene so we can create a new instance of it
    ball_scene = preload("res://Assets/Scenes/ball.tscn")

    # Load (preload) the Smoke Particle scene for visual effects
    particles = preload("res://Assets/Scenes/smoke.tscn")

    # Create an instance of the Ball from the preloaded scene
    ball_instance = ball_scene.instantiate()

    # Get a reference to the Marker3D node in the scene
    marker = $Marker3D

    # Set the Ball's position to the same location as the Marker3D
    ball_instance.position = marker.position

    # Add the ball instance to the current scene as a child
    add_child(ball_instance)
```

_ready 函数在火炮被添加到场景时只调用一次，用于设置火炮的初始状态。下面对这里的代码进行逐行解释：

- 从指定路径加载炮弹场景，并将其存储在 ball_scene 变量中。preload 关键字确保场景在游戏运行时即可使用，并且在使用 GDScript 时相比 load 函数提供了性能提升。
- Ball 场景的一个实例（原始场景的副本）存储在 ball_instance 变量中。
- Marker3D 节点的引用存储在 marker 变量中。
- 新创建的炮弹的位置被设置为与 marker 的位置相匹配。
- 最后，球实例被添加到 Cannon 节点层级（场景树）中，使其在场景中可见并处于活动状态。

3. 为了让火炮以规律的速率发射炮弹，我们需要使用 **Cannonball_timer**。连接 timeout() 信号并添加以下代码：

```
func _on_cannonball_timer_timeout():
    ball = ball_scene.instantiate()
    ball.position = marker.position
    add_child(ball)
```

因为我们将 **Wait Time** 设置为 3 秒，所以 timeout() 函数中的代码每 3 秒运行一次。将在 marker 位置创建一个新的炮弹实例，并将其添加到 **Cannon** 场景的层级中。此计时器确保炮弹从火炮中重复发射。

4. 在 **Level** 场景中实例化 **Cannon** 场景，并测试一切是否按预期工作。

该关卡现在拥有可收集物和障碍物。我们现在应关注胜利条件或目标。为此，我们将放置一面用户必须到达以完成关卡的旗帜。

## 完成我们的关卡

要完成关卡，玩家必须到达 *flag*。旗帜位于最高平台上，唯一到达它的方式是获得 Gem 来增强玩家的跳跃能力，避开炮弹并冲向旗帜。

这些都是设计决策，关卡中的资源让我们可以考虑不同的选项。我们本可以要求玩家获得钥匙来解锁通往旗帜的大门。这就是游戏开发如此令人满意的原因！现在我们应该为旗帜创建一个场景。

## 创建 Flag 场景

在创建 **Flag** 场景之前，确定旗帜应该做什么会很有帮助，因为这有助于我们确定使用哪些节点。在我们的简单案例中，旗帜唯一需要做的就是在玩家触碰到它时退出游戏。当然，如果我们制作更多关卡，同样可以轻松切换关卡。知道这一点，**Area3D** 节点是旗帜的最佳选择。按照以下步骤进行设置：

- 1. 使用 **Area3D** 作为根节点创建一个新场景。
- 2. 将其重命名为 Flag。
- 3. 添加一个 **MeshInstance3D** 节点并使用 `flag.obj` 作为 Mesh 属性。
- 4. 应用 palette.png 来为旗帜贴图。
- 5. 将 **CollisionShape3D** 节点作为旗帜的子节点添加。
- 6. 用 **New BoxShape3D** 包围旗帜。

按照步骤操作后，您的 **Flag** 场景层级将如下图 9.11 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_260_0.png)

图 9.11 – Flag 场景层级

- 7. 我们再次利用 **Area3D** 节点的多功能性来检测 **CharacterBody** 节点（例如玩家）何时进入其空间。为 **Flag** 节点添加一个脚本并将其保存为 flag.gd。
- 8. 现在连接 **Flag** 节点的 _on_body_entered 信号，然后按如下方式编写代码：

```
extends Area3D
func _on_body_entered(body):
    if body.is_in_group("player"):
        get_tree().quit()
```

这段简单的代码测试进入旗帜空间的物体是否在 **player** 组中。如果是，我们就退出游戏。

## 切换场景

创建多个关卡超出了本书的范围；然而，如果你创建了另一个关卡并希望玩家到达旗帜后切换到它，这非常简单。

首先，你需要将第二个关卡创建并保存为场景，例如 level_2.tscn。将场景文件的路径保存在变量中，然后使用内置的 get_tree().change_scene_to_file() 函数切换到下一个场景。代码如下所示：

```
extends Area3D
func _on_body_entered(body):
    if body.is_in_group("player"):
        var next_scene_file =
        "res://scenes/level_2.tscn"
        get_tree().change_scene_to_file(next_scene_file)
```

此代码检查玩家是否进入了特定区域。如果是，则加载游戏的下一关。它使用组来确保只有玩家触发该操作。它使用场景路径来确定要加载哪个关卡。下面逐行解释：

- 当一个物体进入旗帜的区域时
- 代码检查该物体是否在 "player" 组中
- 将关卡 2 场景文件的路径存储在名为 next_scene 的变量中
- 使用 change_scene_to_file 方法切换到已保存的场景

现在玩家有可收集的增益道具、可避免的障碍和需要到达的目标，关卡已可基本游玩。让我们现在着手为关卡添加润色，例如粒子、移动和颜色。

## 润色我们的关卡

创建一个功能性关卡是游戏开发中的重要第一步，但要使其在视觉上吸引人并让玩家沉浸其中，需要额外的努力——这就是关卡润色的作用所在。

润色关卡意味着通过动态视觉、流畅动画和鲜艳效果等元素来增强其外观和手感。这些补充不仅使游戏更具沉浸感，还有助于传达氛围、增添个性并使游戏玩法更令人满意。

在本节中，我们将探讨如何添加移动、颜色和粒子效果为关卡注入活力，将其从基本设计转变为玩家会喜欢的体验。

Godot 提供了一个名为 WorldEnvironment 的节点，用于设置整个场景的默认环境属性，包括后期处理效果、照明和背景设置。

在 Level 1 场景中，添加一个 WorldEnvironment 节点。在 Inspector 中，找到 Environment 属性并选择 New Environment。如图 9.12 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_261_0.png)

图 9.12 – 向世界添加新环境

一旦我们加载了新环境，如图 9.12 所示，我们现在可以访问更多属性和设置。

## 设置背景颜色

通过点击 **Environment**，选择 **Background**，然后选择 **Custom Color**，并找到天蓝色，将关卡的背景颜色更改为天蓝色。如图 9.13 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_262_0.png)

图 9.13 – 为关卡设置自定义背景颜色

现在我们已为关卡添加了背景颜色，如图 9.13 所示，我们应该在每次发射后为火炮添加一些烟雾效果。

## 添加粒子效果

**Particle effects** 是游戏开发中为游戏增添视觉魅力并增强氛围的强大工具。它们可以模拟各种自然现象，如火焰、烟雾、雨水或爆炸，也可以创建抽象效果，如闪光、轨迹或魔法光晕。通过融入粒子效果，你可以让游戏感觉更具动态性和吸引力，帮助玩家更好地与你创造的世界产生联系。在本节中，我们将探讨如何创建和自定义粒子效果以让场景栩栩如生。

## 创建烟雾场景

如前所述，我们将创建一个小烟雾 puff，在每次炮击后升入天空。我们将使用其中一个云模型，并将其缩小并旋转。按照以下步骤创建烟雾场景：

- 1. 使用 **CPUParticles3D** 作为根节点创建一个新场景。
- 2. 将节点重命名为 Smoke。
- 3. 展开 **Drawing** 部分并找到 **Mesh** 属性。
