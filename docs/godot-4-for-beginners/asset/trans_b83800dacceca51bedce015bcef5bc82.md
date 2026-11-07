
animate() 函数负责根据玩家的移动、生命值以及是否被击中在不同的动画状态之间切换。让我们逐一查看每个条件以了解其工作原理。

首先，让我们来看一下空闲状态的代码：

```
if velocity.x == 0 && velocity.y == 0 && hearts > 0 && isHit == false:
    animations.play("idle")
```

如果玩家没有移动（velocity.x == 0 且 velocity.y == 0）、仍然存活（hearts > 0），并且刚刚没有被击中（isHit == false），我们播放 "idle" 动画。

接下来，让我们检查受击状态：

```
elif isHit:
    animations.play("hit")
```

如果玩家被击中，我们播放 "hit" 动画以显示反应。这通常会短暂播放，然后恢复到其他动画。

这就是我们检测向右奔跑的方式：

```
elif velocity.x >= 0 && velocity.y == 0 && hearts > 0:
    animations.play("run")
    animations.flip_h = false
```

如果玩家水平向右移动（x 速度为正）、垂直方向没有移动，并且仍然存活，我们播放 "run" 动画并确保精灵面向右侧（flip_h = false）。

向左奔跑的代码类似：

```
elif velocity.x <= 0 && velocity.y == 0 && hearts > 0:
    animations.play("run")
    animations.flip_h = true
```

这与之前的条件相同，但用于向左移动。我们水平翻转精灵（flip_h = true）使其面向左侧。

以下是坠落的处理方式：

```
elif velocity.y > 0 and !is_wall_sliding && hearts > 0:
    animations.play("fall")
```

如果玩家向下移动（velocity.y > 0）、没有在墙上滑动，并且仍然存活，我们显示 "fall" 动画。

跳跃逻辑如下：

```
elif velocity.y < 0 and jump_count == 0 && hearts > 0:
    animations.play("jump")
```

此条件检查玩家是否向上跳跃（velocity.y < 0）并且是第一次跳跃（jump_count == 0），这会触发 "jump" 动画。

以下是双跳的检查：

```
elif !is_on_floor() and jump_count > 0 and !is_wall_sliding:
    animations.play("double_jump")
```

如果玩家在空中（不在地面上）、已使用至少一次跳跃，并且没有在墙上滑动，我们显示 "double_jump" 动画。

这处理右侧墙壁滑动：

```
elif is_wall_sliding and Input.is_action_pressed("ui_right"):
    animations.flip_h = false
    animations.play("wall_slide")
```

如果玩家在墙上滑动并且按住向右移动键，我们播放 "wall_slide" 动画并使精灵面向右侧。

最后，这就是我们用于左侧墙壁滑动的内容：

```
elif is_wall_sliding and Input.is_action_pressed("ui_left"):
    animations.flip_h = true
    animations.play("wall_slide")
```

它与右侧墙壁滑动相同，但用于左侧。

一切几乎按我们想要的方式工作。然而，你会注意到玩家卡在受击动画中，这是因为没有事件将 isHit 变量重置为 false。最后一步是返回到 Mushroom 场景，选择 HurtPlayerZone 节点，选择 Node 选项卡，然后选择 Signals 选项卡，并连接 body_exited() 信号。按如下所示编写信号函数：

```
func _on_hurt_player_zone_body_exited(body: Node2D):
    if body.is_in_group("Player"):
        body.isHit = false
```

为了保持一致，我们还应该从 Mushroom 脚本中调用 isHit 函数：

```
func _on_hurt_player_zone_body_entered(body):
    if "Player" in body.name:
        body.hurt()
        body.isHit = true
```

前面的代码确保如果退出 HurtPlayerZone Area2D 节点的物体属于名为 "Player" 的组，则 hit 变量将被重置为 false。这意味着当玩家停止与蘑菇碰撞时，受击动画也会停止。

现在，一切就绪，可以在玩家游戏时提供令人满意的视觉反馈，以提醒他们处于危险之中。

我们的下一步是为检查点旗帜添加一个有趣的视觉效果。我们将发射纸屑大炮，以小规模庆祝完成关卡。

## 创建纸屑大炮效果

为了强化成就感并作为完成关卡的额外奖励，当玩家触碰检查点旗帜时，我们将从旗杆上发射一些多彩纸屑。这将以粒子效果的形式出现，我们将将其作为独立场景设置。让我们开始吧！

我们将首先创建一个以 Node2D 为根节点的新场景。将节点重命名为 Confetti_Cannon，添加一个 GPUParticles2D 节点作为子节点，并将其重命名为 Blue，因为它代表蓝色纸屑。如下图所示 10.9 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_286_0.png)

图 10.9 – Confetti_Cannon 节点和纸屑粒子

现在，选择 Blue 纸屑粒子节点。在 Inspector 中，找到 Process Material 设置，然后添加 New ParticleProcessMaterial，如图 10.10 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_287_0.png)

图 10.10 – 创建新的粒子过程材质

接下来，点击 ParticleProcessMaterial（文字）以展开粒子的设置菜单。

纸屑在飞行时应旋转，但不应远离玩家。在 Particle Flags 设置中启用 Rotate Y 并禁用 Z，如图 10.11 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_287_1.png)

图 10.11 – 粒子标志设置

纸屑应沿弧线移动，以模仿现实中纸屑的行为。为此，调整纸屑生成的角度。你可以根据需要调整数值——我将最小角度设置为 209，最大角度设置为 327，如图 10.12 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_288_0.png)

图 10.12 – 设置纸屑的最小和最大生成角度

确保将 Direction 变量设置为 x = 0.0、y = -1.0 和 z = 0.0，如图 10.13 所示。

纸屑粒子还应分散开来，而不是全部沿同一路径前进。为了在一定角度范围内分散它们，我选择了 Spread 值为 100，这将为每个新粒子选择介于 −100 到 100 度之间的角度。

此外，由于纸屑通常是爆炸性事件，我们希望改变粒子的 Initial Velocity 值。两个更改均显示在图 10.13 中：

![](img/d34207e07c133a9e84d9caf1d479006c_288_1.png)

图 10.13 – 改变纸屑粒子的 Spread 和 Initial Velocity 设置

为了使效果更令人印象深刻，我们可以改变每个粒子在关卡中移动时的旋转速率。这可以通过改变 Angular Velocity 值来实现。我已将速率锁定在 280，如图 10.14 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_289_0.png)

图 10.14 – 设置粒子的角速度可变速率

纸屑还应以恒定速率从中心加速远离，我们可以轻松地在线性加速度设置（即 Linear Accel）中设置此项，如图 10.15 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_289_1.png)

图 10.15 – 设置粒子的线性加速度

为了防止纸屑离旗帜太远，我们应在 Accelerations 部分增加 Gravity，如图 10.16 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_289_2.png)

图 10.16 – 增加粒子在 y 方向的重力

要控制纸屑粒子的外观，我们前往 **ParticlesProcessMaterial** 的 **Display** 属性。现在，改变纸屑粒子的 **Scale** 值——我将最小尺寸设置为 5，最大尺寸设置为 20。对于 **Color**，我选择了 *天蓝色*。这些更改可在 *图 10.17* 中看到：

![](img/d34207e07c133a9e84d9caf1d479006c_290_0.png)

*图 10.17 – 设置粒子的尺寸变化和颜色*

下一步是控制这些粒子在屏幕上停留的时间以及它们在存活期间的行为。这些设置的 **Time** 属性中可以找到。默认生命周期为 1 秒，这很好；然而，我们必须打开 **One Shot**，以便纸屑只发射一次且不会持续生成。你可以改变 **Explosiveness** 和 **Randomness**，或使用 *图 10.18* 中显示的值：

![](img/d34207e07c133a9e84d9caf1d479006c_290_1.png)

*图 10.18 – 调整粒子的 Time 属性*
