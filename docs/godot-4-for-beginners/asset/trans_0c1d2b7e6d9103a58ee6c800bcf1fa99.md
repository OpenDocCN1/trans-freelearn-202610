
10. 在图 9.6 所示的方式将计时器的 timeout() 信号连接到 Gem 脚本后，添加以下代码：

在第 23 行添加一个名为 default_jump_strength 的新变量，并将其值设为 7：

```
var default_jump_strength = 7
```

现在，按如下所示编写超时函数：

```
func _on_jump_cooldown_timer_timeout():
    jump_strength = default_jump_strength
```

这段代码将在计时器达到持续时间（等待时间）后将跳跃强度重置为默认值。

💡 快速提示：使用 AI 代码解释器和快速复制功能提升您的编码体验。在 next-gen Packt Reader 中打开本书。点击复制按钮 (1) 快速将代码复制到您的编码环境，或点击解释按钮 (2) 让 AI 助手为您解释一段代码。

![](img/d34207e07c133a9e84d9caf1d479006c_252_0.png)

![](img/d34207e07c133a9e84d9caf1d479006c_252_1.png)

🔒 next-gen Packt Reader 随本书购买免费提供。扫描二维码或访问 packtpub.com/unlock，然后使用搜索栏按名称查找本书。仔细核对所示版本，确保获取正确的版本。

![](img/d34207e07c133a9e84d9caf1d479006c_252_2.png)

11. 在离开 Player 脚本之前，我们应该为计时器添加一个引用变量，以便我们可以从 Gem 场景启动它。在 MeshInstance3D 的 onready 变量下方，添加计时器的变量，如下所示：

```
@onready var model = $MeshInstance3D
@onready var jump_cooldown_timer = $Jump_cooldown_timer
```

12. 在 Player 脚本中，添加一个用于道具跳跃强度的变量：

```
var powerup_jump_strength = 10
```

现在添加一个函数，在 Gem 被收集时增加跳跃强度并启动计时器：

```
func on_gem_collected():
    jump_cooldown_timer.start()
    jump_strength = powerup_jump_strength
```

13. Player 场景现在是 player 组的一部分。它还设置了计时器来计时增加的跳跃强度持续时间，并且我们已提供对计时器的访问。现在返回 Gem 场景以完成 body_entered() 函数：

```
func _on_area_3d_body_entered(body):
    if body.is_in_group("player"):
        body.on_gem_collected()
        queue_free()
```

当有物体进入 Gem 区域时，这段代码将被触发。如果该物体属于 player 组，则该物体（玩家）会将 jump_strength 增加到 10。物体的 jump_cooldown 计时器会启动并运行直到超时，queue_free() 确保 Gem 将从场景中移除。

14. 在 Level 场景中实例化一个 Gem 场景（在 Level 场景中按 Ctrl + Shift + A，然后选择 Gem 场景），并测试玩家在收集 Gem 后现在可以到达最高平台。您现在可以围绕关卡策略性地放置 Gems。

玩家以 Gem 的形式获得奖励，Gem 会增加跳跃强度。现在我们将在关卡中添加一个障碍，使奖励没有那么容易获得。

## 介绍障碍

障碍通过创建玩家必须绕过或克服的危险，为关卡引入挑战和刺激。它们鼓励玩家制定策略、提高技能，并在游戏中不断保持参与。随着游戏进程，障碍可以采用多种形式，例如移动平台、尖刺或环境陷阱，并可为游戏玩法增添多样性。在本节中，我们将在关卡中添加一些障碍来测试玩家的敏捷性和时机把握，创造更动态和更具奖励性的体验。

## 创建 Cannon 场景

一个有趣且动态的障碍是添加一门大炮，定期发射子弹，玩家必须在到达检查点之前避开它。大炮是静态的，纯粹是装饰性的或布景。然而，子弹将从大炮移动，穿过关卡。按照以下步骤设置 Cannon 场景：

- 1. 使用 Node3D 节点作为根节点创建一个新场景。将此重命名为 Cannon。
- 2. 添加一个 StaticBody3D 节点作为子节点。
- 3. 添加一个 MeshInstance3D 节点并使用 cannon.obj 作为 Mesh 属性。
- 4. 记得将 palette.png 拖到大炮对象以进行纹理贴图。
- 5. 添加一个 CollisionShape3D 节点并使用新的 BoxShape3D 属性将大炮包围。
- 6. 添加一个 Timer 节点并将其重命名为 Cannonball_timer。
- 7. 添加一个 Marker3D 节点并将其放置在大炮的炮口/开口处：x 位置 0.0，y 位置 0.5，z 位置 -1.0。

按照这些步骤，Cannon 场景树应如下图所示 9.7：

![](img/d34207e07c133a9e84d9caf1d479006c_254_0.png)

图 9.7 – Cannon 场景的场景树

图 9.7 中的组件将用于使大炮发挥功能。它是一个静态体，因为它不会移动，但玩家可以跳到上面并站在上面。大炮弹计时器将定期生成炮弹，这些炮弹在关卡中移动。标记用于精确定位炮弹应出现的位置。

我们希望大炮每三秒发射一颗球。为此，调整 **Cannonball_timer** 节点的设置，如图 9.8 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_255_0.png)

图 9.8 – 炮弹计时器的设置

使用图 9.8 所示的设置，大炮将每三秒自动发射一颗球。如果您想更改发射频率，请调整 **等待时间**。

大炮现在已准备好进行脚本编写；然而，我们仍然需要一个球或子弹供其发射。我们现在将准备 **Ball** 场景，以便从大炮发射投射物。

## 创建 Ball 场景

创建一个新场景并使用 **CharacterBody3D** 节点作为根节点。将它重命名为 Ball。

需要注意的是，我们通常会为投射物使用 **Area3D** 节点，因为这允许我们检测碰撞并触发事件，例如在撞击时应用伤害或销毁投射物。然而，在我们的游戏中，炮弹仅作为物理障碍物——它不会伤害玩家，但如果接触到玩家则应简单地将其推开。

按照以下步骤完成 Ball 场景：

- 1. 您已经使用 **CharacterBody3D** 节点作为根节点创建了一个新场景。
- 2. 您已将根节点重命名为 **Ball**。
- 3. 添加一个 **MeshInstance3D** 节点作为子节点并使用 Bullet.obj 作为 Mesh 属性。
- 4. 将 palette.png 应用为子弹对象的纹理。
- 5. 添加一个 **CollisionShape3D** 节点并用新的 **CapsuleShape3D** 包围子弹。
- 6. 在 *x* 轴上旋转 **CapsuleShape3D** 并调整其大小以 neat 地贴合模型。
- 7. 添加一个 **Timer** 节点并将其重命名为 `Destroy_timer`。

按照这些步骤，**Ball** 场景树应如下图所示 *图 9.9*：

![](img/d34207e07c133a9e84d9caf1d479006c_256_0.png)

*图 9.9 – Ball 场景的组件*

*图 9.9* 中看到的一个重要组件是球的 **Destroy_timer** 节点。它代表球的生命周期。一旦超时，我们销毁球并将场景从关卡中移除。将计时器的设置调整为 *图 9.10* 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_256_1.png)

*图 9.10 – 用于销毁球的计时器设置*

*图 9.10* 将球的生命周期设置为 5 秒；计时器自动启动，但每个球仅运行一次。球需要在关卡中穿行并被销毁。为此，我们将编写脚本。

## 编写 Ball 脚本

没有炮弹，大炮用处不大——所以让我们用脚本赋予它们生命，该脚本处理它们的移动和碰撞。请按照以下步骤操作：

- 1. 将脚本附加到 **Ball** 节点。设置一个 speed 变量并赋值为 3。这是一个导出变量，以便您在测试游戏时可以更改此值，从而找到最适合您游戏的值：
- 2. 在 process 函数中，我们将沿 z 轴移动球。这是深度轴，表示球朝向或远离我们进入或离开屏幕移动。为了平滑移动球，我们将通过 *相乘* speed 与 delta 来改变位置，如下所示：
- 3. 连接 **Destroy_timer** 的 **timeout()** 信号，并在其中编写代码，使球在达到计时器的等待时间后被销毁：

```
@export var speed = 3
```

```
func _process(delta):
    position.z -= speed * delta
```

```
func _on_destroy_timer_timeout():
    queue_free()
```

有了这个脚本，炮弹可以完成我们想要的两个任务。它可以在关卡中移动，并在一定时间后被销毁。现在为大炮编写脚本以生成炮弹。

## 编写 Cannon 脚本

要编写 Cannon 脚本，首先，我们将脚本附加到 **Cannon** 节点。在整本书中，我对应在场景首次创建时初始化的变量使用了 @onready var。这次，我将向您展示使用 _ready 函数初始化变量的替代快捷方式。让我们在这里看看：

- 1. 我们将实例化 **Ball** 场景（生成一个球），因此我们需要一个场景变量来存储对 **Ball** 场景的引用。我们还需要一个变量来表示球本身（ball 变量），它将是场景的一个实例。我们还需要一个引用变量（marker 变量）指向 **Marker3D** 节点，以便我们可以将球定位到不可见标记所在的位置：

```
var ball_scene
var ball_instance
var marker
```
