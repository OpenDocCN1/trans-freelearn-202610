
图 10.3 – 表示玩家生命值变化的心形帧

![](img/d34207e07c133a9e84d9caf1d479006c_277_0.png)

图 10.3 – 表示玩家生命值变化的心形帧

## 资源

如果你不习惯自己绘制爱心，可以使用 Shlok Gupta 创建的爱心。然而，你仍需要对其进行修改，以便在不同生命值水平下显示爱心。它可在他们的 Itch 页面在此处获取：https://gamedevshlok.itch.io/heartpack.

当你的爱心图像准备就绪后，转到名为 HUD 的 AnimatedSprite2D 节点的 Sprite Frames 属性，并选择 New SpriteFrames，如图 10.4 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_277_1.png)

图 10.4 – AnimatedSprite2D 节点的 New SpriteFrames

现在，单击属性中的单词 SpriteFrames 以打开 Animation Frames 窗口。将默认动画重命名为 idle 并添加四个帧，如图 10.5 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_278_0.png)

图 10.5 – 生命条的 idle 动画有四个帧表示生命值总数

如图 10.5 所示，每当玩家失去生命值时，我们将更改动画帧以匹配。我们不需要为生命条添加脚本；我们只需要在附加到 level1.tscn 的脚本中引用它即可。

在 Level1 场景中，将你新建的 hud.tscn 拖入你的关卡并将其放置在左上角，如图 10.6 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_278_1.png)

图 10.6 – HUD 的一个好位置

将 HUD 放置到场景中后，你的 Level1 场景树应包含 HUD 场景，并看起来像图 10.7 所示的场景层次结构：

![](img/d34207e07c133a9e84d9caf1d479006c_279_0.png)

图 10.7 – level 1 的场景层次结构

如图 10.7 所示场景就位后，是时候打开附加到 Level1 的脚本并进行一些添加，以便 HUD 根据玩家拥有的生命值更新爱心。

## 在 Level 脚本中更新 HUD

现在我们的基于爱心的生命系统已经就位，我们需要确保玩家的当前生命值在屏幕上得到反映。为此，我们将从 Level 脚本更新 HUD。HUD 将根据玩家剩余的爱心数量在视觉上发生变化，随着受到伤害，爱心会变灰。

我们将首先添加一个 @onready 变量来引用 HUD 场景。在 _process() 函数中，我们将更新 HUD 的 frame 属性以匹配玩家的当前生命值。这决定了显示为活动（红色）与非活动（灰色）的爱心数量。例如，如果玩家有三个爱心，我们将把 HUD 帧设置为 3 以显示三个红色爱心。

以下是更新后的 Level1 脚本：

```gdscript
extends Node2D

@onready var update_health_hud = $HUD
@onready var player = $Player
@onready var strawberries = $Collectibles

func _ready():
    player.level_strawberries = strawberries.get_child_count()

func _process(delta):
    update_health_hud.frame = player.get_hearts()
```

让我们分解代码以更好地理解它：

- update_hud 存储对 HUD 节点的引用，以便我们可以轻松访问和更新它。
- 在 _process() 中，每帧运行一次，我们将 HUD 的帧设置为反映玩家的当前爱心数量。这意味着当玩家获得或失去生命值时，HUD 将实时视觉更新。

player.get_hearts() 行依赖于我们需要在 Player 脚本内部创建的新函数。该函数仅返回玩家当前拥有的爱心数量：

```gdscript
func get_hearts():
    return hearts
```

为此支持，在 Player 脚本顶部添加一个新变量来存储玩家的生命值：

```gdscript
var hearts = 3
```

问题是：我们为什么要添加这个变量？

hearts 变量跟踪玩家拥有的生命值。我们将其初始化为 3，意味着玩家以满生命值开始。随着玩家受到伤害，此值会减少。

接下来，让我们创建一个 hurt() 函数，在玩家受伤时减少其生命值：

```gdscript
func hurt():
    hearts -= 1
    if hearts <= 0:
        death()
```

此函数在玩家受伤时减去一个爱心。如果 hearts 达到 0，我们将触发 death() 函数，我们现在将在以下代码中创建它：

```gdscript
func death():
    #shoot the player upward upon death
    velocity.y = -600
```

```gdscript
#disable collisions so the player falls through the level
set_collision_mask_value(2, false)
set_collision_mask_value(3, false)
```

以下是前面代码的分解：

- velocity.y = -600 在玩家死亡时将其向上射出，增加一点手感使死亡感觉更戏剧化
- 我们通过关闭关卡的碰撞层来禁用与环境的碰撞，允许玩家从屏幕上掉落

另一个提供出色视觉反馈给玩家的游戏手感——帮助他们快速识别何时受到伤害——是受击闪烁效果。这种常见技术会短暂改变玩家的外观（通常为白色或红色），以示其已受伤，使体验感觉更灵敏和精致。

## 添加受击动画

在本节中，我们将创建一个简单的动画，当玩家受伤时使其短暂变白。这种效果不仅使受损时刻更具戏剧性，还帮助玩家保持对当前生命状态的了解，尤其是在快节奏的时刻。

在你的文件资源管理器中，定位 Pixel Adventure/Main Characters/Ninja Frog 文件夹（你可以在此处下载所有资源：https://pixelfrog-assets.itch.io/）并将 Hit.png 文件拖入 Godot FileSystem。

在 Player 场景中，选择 AnimatedSprite2D 节点并添加一个名为 hit 的新动画。现在，单击 Add Frames from Sprite Sheet 按钮并将其设置为 7 个水平帧和 1 个垂直帧。然后，选择所有帧并导入它们。将动画速度设置为 16 FPS 且不循环。受击动画将如 图 10.8 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_281_0.png)

🔍 **快速提示**：需要查看此图像的高分辨率版本吗？在 next-gen Packt Reader 中打开本书或在 PDF/ePub 副本中查看。

🔒 **下一代 Packt Reader** 和一本 **免费的 PDF/ePub 副本** 包含在你的购买中。扫描二维码或访问 packtpub.com/unlock，然后使用搜索栏按名称查找本书。仔细检查所示的版本，以确保你获得正确的版本。

![](img/d34207e07c133a9e84d9caf1d479006c_282_0.png)

![](img/d34207e07c133a9e84d9caf1d479006c_282_1.png)

为确保我们的动画在需要时运行，在 Player 脚本顶部 vars 组的变量中添加一个名为 isHit 的辅助变量。它将是一个布尔值，默认为 false：

```gdscript
var isHit = false
```

现在，修改 hurt() 函数，以便在玩家受到伤害时将 isHit 设置为 true：

```gdscript
func hurt():
    isHit = true
    hearts -= 1
    if hearts <= 0:
        death()
```

现在，我们可以修改 animate() 函数以包含对玩家拥有的爱心数量的检查，以及是否已被击中：

```gdscript
func animate():
    if isHit:
        animations.play("hit")
        await animations.animation_finished
        animations.play("idle") # transition back to idle
        return # Prevent further changes this frame
    if hearts == 0:
        return  # Dead, don't animate further

    # Stationary player
    if velocity.x == 0 and velocity.y == 0:
        animations.play("idle")

    # Player moving right
    elif velocity.x >= 0 and velocity.y == 0:
        animations.play("run")
        animations.flip_h = false

    # Player moving left
    elif velocity.x <= 0 and velocity.y == 0:
        animations.play("run")
        animations.flip_h = true

    # Player is falling
    elif velocity.y > 0 and !is_wall_sliding:
        animations.play("fall")

    # Player is jumping (initial jump)
    elif velocity.y < 0 and jump_count == 0:
        animations.play("jump")

    # Player is double-jumping
    elif !is_on_floor() and jump_count > 0 and !is_wall_sliding:
        animations.play("double_jump")

    # Player is wall sliding on the right
    elif is_wall_sliding and Input.is_action_pressed("ui_right"):
        animations.flip_h = false
        animations.play("wall_slide")

    # Player is wall sliding on the left
    elif is_wall_sliding and Input.is_action_pressed("ui_left"):
        animations.flip_h = true
        animations.play("wall_slide")
```
