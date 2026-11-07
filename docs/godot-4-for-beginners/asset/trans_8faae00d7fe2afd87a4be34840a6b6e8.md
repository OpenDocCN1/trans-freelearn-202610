
我们玩家的最终自定义函数将处理着陆时的重生和动画效果。按所示代码创建 handle_respawn() 函数：

```
func handle_respawn(delta):
    var border_position_y = -10
    # Threshold below which the player respawns
    if position.y < border_position_y:
        get_tree().reload_current_scene()

    # Animation for scale (jumping and landing)
    model.scale = model.scale.lerp(Vector3(1, 1, 1), delta
    * smoothing_factor)

    # Animation when landing
    if is_on_floor() and vertical_velocity > 2 and
    !previously_floored:
        model.scale = Vector3(1.25, 0.75, 1.25)

    previously_floored = is_on_floor()
```

让我们研究 handle_respawn() 函数：

- **场景重载（重生）：** 如果玩家的位置.y 低于 -10（意味着他们已掉出关卡），我们调用 get_tree().reload_current_scene() 来重新加载当前场景并使玩家在起始位置重生。
- **重置缩放（恢复大小）：** 我们使用 model.scale.lerp(Vector3(1, 1, 1), delta * smoothing_factor) 在跳跃或着陆产生的挤压和拉伸效果后，逐渐将玩家的缩放插值回正常状态。
- **着陆动画（挤压效果）：** 如果玩家当前在地面上（is_on_floor()），下落速度很快（gravity > 2），并且在上一帧不在地面上（!previously_floored），我们通过将 model.scale 改为 Vector3(1.25, 0.75, 1.25) 触发挤压效果。
- **地面状态跟踪：** 最后，我们更新 previously_floored 变量以跟踪玩家在上一帧是否在地面上。

为了让所有这些机制平稳运行，我们需要在 Godot 的主物理循环中调用自定义函数：

```
func _physics_process(delta):
    handle_controls(delta)
    handle_gravity(delta)
    handle_movement(delta)
    handle_rotation(delta)
    handle_respawn(delta)
```

_physics_process(delta) 函数是 Godot 用于物理计算的内置循环，每帧物理帧调用一次（默认每秒 60 次）。它确保我们的游戏逻辑在任何帧率下都能一致运行。通过在此处调用自定义函数，我们可使重力、移动、跳跃、摄像机效果和重生与物理引擎保持同步。

现在我们的角色控制器已能正常工作——处理移动、跳跃和着陆动画——是时候设置摄像机，使其在 3D 空间中正确跟随玩家了。配置良好的摄像机通过帮助玩家保持方向感和专注于周围环境来提升游戏体验。在下一节中，我们将把摄像机连接到玩家并编写脚本，使其能平滑响应玩家的移动和旋转。

## 实现摄像机控制器

开始配置摄像机时，我们将先创建一个新场景作为摄像机控制器，然后将其链接到玩家。

- 1. 创建一个新场景并添加一个 Node3D 节点作为根节点。将此节点重命名为 View。现在添加一个 Camera3D 节点作为子节点。场景树如图 8.20 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_239_0.png)

图 8.20 - View 的场景组成

- 2. 选择 Camera3D 节点，并在 Inspector 中将 Current 属性设置为 On——标记此摄像机为要使用的摄像机。你还可以通过将 FOV 属性设置为 40 来调整视野范围。如图 8.21 所示：

![](img/d34207e07c133a9e84d9caf1d479006c_240_0.png)

图 8.21 – 将摄像机设为当前并调整 FOV

- 3. 现在，将脚本附加到 View 节点并将其保存为 view.gd。确保将模板设置为 Empty。

现在我们准备开始构建摄像机控制器脚本，该脚本将允许玩家使用键盘上的按键（我们在输入映射中定义）移动摄像机并调整缩放。

摄像机需要一个目标节点来注视或聚焦。这将是我们的 Player 节点。我们将按以下代码所示将此目标导出为变量：

```
@export_group("Properties")
@export var target: Node
```

此导出的 target 变量将出现在 Inspector 中，允许我们将 Player 节点拖放到其中。摄像机随后将知道它应该跟随什么。

现在导出变量以控制摄像机的缩放级别和旋转速度：

```
@export_group("Zoom")
@export var zoom_minimum = 16
@export var zoom_maximum = 4
@export var zoom_speed = 10
```

这些变量定义了缩放行为：zoom_minimum 和 zoom_maximum 决定摄像机可以缩放的进出范围。此外，zoom_speed 控制在触发输入时缩放发生的速度。

接下来，让我们定义 rotation_speed 变量：

```
@export_group("Rotation")
@export var rotation_speed = 120
```

此变量设置摄像机在玩家旋转或使用输入转动摄像机时围绕玩家旋转的速度。

我们将使用两个标准变量来跟踪摄像机的当前旋转和缩放：

```
var camera_rotation: Vector3
var zoom = 10
```

让我们进一步探讨：

- camera_rotation 将存储摄像机的当前旋转角度（以度为单位）
- zoom 跟踪当前缩放值，我们将根据玩家输入随时间修改它

最后，我们需要对场景中实际的 Camera3D 节点的引用，以便我们可以操作它：

```
@onready var camera = $Camera3D
```

使用 @onready 可确保此变量在节点添加到场景并准备好使用时设置一次。

在内置的 _ready() 函数中，我们将存储摄像机的初始旋转：

```
func _ready():
    camera_rotation = rotation_degrees # Initial rotation
```

这使用摄像机的当前旋转角度初始化 camera_rotation 变量，以便我们可以从已知的起点更新和修改它。

与玩家类似，摄像机也可以在场景中移动。让我们创建一个函数来处理摄像机输入：

```
func handle_input(delta):
    # Rotation
    var input := Vector3.ZERO
    input.y = Input.get_axis("camera_left", "camera_right")
    input.x = Input.get_axis("camera_up", "camera_down")

    camera_rotation += input.limit_length(1.0) *
    rotation_speed * delta
    camera_rotation.x = clamp(camera_rotation.x, -80, -10)

    # Zooming
    zoom += Input.get_axis("zoom_in", "zoom_out") *
    zoom_speed * delta
    zoom = clamp(zoom, zoom_maximum, zoom_minimum)
```

handle_input(delta) 函数根据玩家的输入处理摄像机旋转和缩放。水平和垂直旋转存储在名为 input 的变量中，基于用户按下的键。

接下来，摄像机旋转根据此输入进行更新。输入限制为最大 1.0，我们将其乘以 rotation_speed 变量和 delta 以使一切平滑。最后，我们将垂直旋转限制在 -80 到 -10 度之间。这是为了避免过度倾斜。

对于缩放，我们根据玩家输入更新 zoom 变量，然后将其乘以缩放速度和 delta 以确保平滑缩放。同样，我们将缩放级别限制在预定义的最小和最大缩放值之间。

接下来，_physics_process 函数需要按如下方式实现：

```
func _physics_process(delta):
    # Set position and rotation to targets
    self.position = self.position.lerp(target.position,
    delta * 4)
    rotation_degrees =
    rotation_degrees.lerp(camera_rotation, delta * 6)

    camera.position = camera.position.lerp(Vector3(0, 0,
    zoom), 8 * delta)

    handle_input(delta)
```

再次使用线性插值使摄像机位置平滑地朝向目标（玩家）移动。

我们乘以 delta * 4 来控制重新定位的速度。

现在当前旋转使用线性插值平滑地朝向期望旋转移动，并乘以 delta * 6 来控制速度。

为了实现平滑的缩放效果，我们沿 z 轴移动摄像机位置以匹配缩放级别。我们乘以 delta * 8 来控制速度。

最后，我们调用 handle_input(delta) 函数，该函数处理玩家的输入并控制摄像机的移动。

虽然我们希望一切按预期运行，但我们需要对其进行测试、修正，然后再次测试。这使得游戏开发成为一个迭代过程，我们进行更改，测试以查看更改是否产生预期结果，然后继续如此。因此，我们的下一步是测试我们迄今为止所做的工作。
