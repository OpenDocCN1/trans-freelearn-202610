

```dart
Vector2 gridOffsetToWorld(Offset gridOffset) {
    double x = (gridOffset.dx * TILE_SIZE);
    double y = (gridOffset.dy * TILE_SIZE);
    return Vector2(x, y);
}
```

这里，我们有两个用于在世界坐标和网格坐标之间进行转换的函数，同时考虑到我们的瓦片大小为 32 x 32。

接下来，我们将更新 `George` 类，以便在移动到触摸位置时更新我们的代码，首先使用 A* 算法计算路径，然后沿路径导航，并在导航过程中改变面朝方向。

- 2. 打开 `george.dart` 文件并向文件中添加以下导入：

```
import 'package:goldrush/utils/map_utils.dart';
import 'package:a_star_algorithm/a_star_algorithm.dart';
```

- 3. 接下来，在类顶部添加以下变量，以跟踪障碍物偏移位置、到目标的路径以及当前路径步骤：

```
List<Offset> barrierOffsets;
List<Offset> pathToTargetLocation = [];
int currentPathStep = -1;
```

- 4. 现在，让我们更新构造函数以传入 `barrierOffsets`：

```
George({required this.barrierOffsets, required this.hud, required Vector2 position, required Vector2 size, required double speed}) : super(
    position: position, size: size, speed: speed) {
```

- 5. 接下来，让我们更新 `moveToLocation` 函数以设置新变量：

```
void moveToLocation(TapUpInfo info) {
    pathToTargetLocation = AStar(
    rows: 50,
    columns: 50,
    start: worldToGridOffset(position),
    end: worldToGridOffset(info.eventPosition.game),
    withDiagonal: true,
    barriers: barrierOffsets
).findThePath().toList();
targetLocation = info.eventPosition.game;
faceCorrectDirection();
```

由于 `pathToTargetLocation[0]` 与当前位置相同，我们将 `currentPathStep` 设置为下一步，即 1：

```
currentPathStep = 1;
targetLocation = gridOffsetToWorld(
    pathToTargetLocation[currentPathStep]);
targetLocation.add(Vector2(16, 16));
movingToTouchedLocation = true;
}
```

这里，我们将 A* 算法的结果存储在 `pathToTargetLocation` 变量中。

我们传入行数和列数，与我们的地图大小相同，即 `50 x 50`。我们将 `withDiagonal` 值设置为允许路径走捷径，这样看起来更自然。你可以在运行此代码时尝试将此值设置为 `false`，以查看差异并决定你的偏好。我们传入 `barrierOffsets`，它将通过构造函数传入。最后，我们将乔治的位置和触摸位置转换为网格偏移坐标。结果将是一条避开水障碍物到达该位置的路径。

如果你将我们在本章“_技术要求_”部分下载的更新地图加载到 Tiled 应用程序中，你会发现我们在地图中心放置了水障碍物，可以用来测试路径导航是否正常工作。

在获取 A* 结果后，我们将 `targetLocation` 设置为触摸位置，然后调用一个我们即将定义的新函数，名为 `faceCorrectDirection`。这个新函数确保当乔治开始导航到目标位置的路径时，面朝方向正确。

接下来，我们将 currentPathStep 设置为从 1 开始，而不是从列表的位置 0 开始。这是因为，当我们得到算法结果时，它会将我们当前位置插入位置 0，而由于我们已经在该位置，因此不需要移动过去！

然后，我们根据 currentPathStep 处的偏移量设置 targetLocation，并将其转换回世界坐标以供我们移动。请记住，我们需要使用网格坐标才能让算法工作，但我们需要世界坐标来以实际像素进行移动。最后，我们向 targetLocation 添加一个向量 16, 16。

这是因为，当转换为世界坐标时，我们是基于瓦片的左上角，但我们希望根据瓦片中心来移动乔治，因此我们向向量的 x 和 y 值添加 16, 16，即瓦片大小 32 的一半。

最后，在此函数中，我们将 movingToTouchedLocation 设置为 true，这将使乔治在 update 函数中开始向 targetLocation 移动。

- 6. 接下来，我们需要更改 update 函数以沿路径行走。此函数允许我们通过在每个路径步骤之间行走来更改下一个路径步骤，直到到达路径的最终点。

在上一步中，我们引用了一个名为 faceCorrectDirection 的新函数，当我们更改 update 函数时也会再次使用它，所以让我们先创建它。

在 update 函数下方，创建一个名为 faceCorrectDirection 的新函数并添加以下代码：

```
void faceCorrectDirection() {
    var angle = getAngle(position, targetLocation);
    if ((angle > 315 && angle < 360) || (angle > 0 &&
    angle < 45) ) { // Facing right
    animation = rightAnimation;
    currentDirection = Character.right;
}
else if (angle > 45 && angle < 135) {
    // Facing down
    animation = downAnimation;
    currentDirection = Character.down;
    }
    else if (angle > 135 && angle < 225) {
        // Facing left
        animation = leftAnimation;
        currentDirection = Character.left;
    }
    else if (angle > 225 && angle < 315) {
        // Facing up
        animation = upAnimation;
        currentDirection = Character.up;
    }
}
```

- 7. 最后在此步骤中，我们将更改 `update` 函数，以便在每个路径步骤采取后更改 `targetLocation`，直到到达我们的触摸位置。

由于 `update` 函数现在相当大，我们将完全重构此函数，以便更轻松地管理代码。

我们将为每种移动类型创建三个新函数，`moveByJoystick`、`moveByKeyboard` 和 `moveByTouch`。我们将 `update` 函数的相关部分移入前两个函数，但对于第三个函数，我们将进行重写，因为我们现在正在绕过障碍物使用路径。

在 `update` 函数下方，添加以下三个空函数存根以开始：

```
void moveByJoystick(double dt) async {}
void moveByKeyboard(double dt) async {}
void moveByTouch(double dt) async {}
```

- 8. 接下来，将代码（可从 https://github.com/PacktPublishing/Building-Games-with-Flutter/blob/main/chapter10/lib/components/george.dart 获取）从 `update` 函数添加到 `moveByJoystick` 函数。

- 9. 接下来，将以下代码从 `update` 函数添加到 `moveByKeyboard` 函数：

```
movePlayer(dt);
playing = true;
movingToTouchedLocation = false;
if (!isMoving) {
isMoving = true;
audioPlayerRunning = await FlameAudio.loopLongAudio(
'sounds/running.wav', volume: 1.0);
}
if (keyUpPressed && (keyLeftPressed ||
keyRightPressed)) {
animation = upAnimation;
currentDirection = Character.up;
} else if (keyDownPressed && (keyLeftPressed ||
keyRightPressed)) {
animation = downAnimation;
currentDirection = Character.down;
} else if (keyLeftPressed) {
animation = leftAnimation;
currentDirection = Character.left;
} else if (keyRightPressed) {
animation = rightAnimation;
currentDirection = Character.right;
} else if (keyUpPressed) {
animation = upAnimation;
currentDirection = Character.up;
} else if (keyDownPressed) {
animation = downAnimation;
currentDirection = Character.down;
} else {
animation = null;
}
```

- 10. 接下来，将以下代码从 update 函数添加到 moveByTouch 函数：

```
if (!isMoving) {
isMoving = true;
audioPlayerRunning = await FlameAudio.loopLongAudio(
'sounds/running.wav', volume: 1.0);
}
movePlayer(dt);
double threshold = 2.0;
var difference = targetLocation - position;
if (difference.x.abs() < threshold &&
difference.y.abs() < threshold) {
if (currentPathStep < pathToTargetLocation.length - 1) {
    currentPathStep++;
    targetLocation = gridOffsetToWorld(
        pathToTargetLocation[currentPathStep]);
    targetLocation.add(Vector2(16, 16));
} else {
    stopAnimations();
    audioPlayerRunning.stop();
    isMoving = false;
    movingToTouchedLocation = false;
    return;
}
}
playing = true;
if (currentPathStep <= pathToTargetLocation.length) {
    faceCorrectDirection();
}
}
```

在 `moveByTouch` 函数中，我们更改了一些内容——所以让我们逐一了解与前面章节讨论过的之前触摸代码不同的部分。

我们仍然检查移动玩家后的位置是否在阈值内，但我们将 `threshold` 增加到了 `2.0`，这有助于触摸移动，尤其是在角色奔跑时。如果我们到达了 `targetLocation`，我们会检查是否有更多步骤，如果还有更多步骤，则增加 `currentPathStep`，然后将 `targetLocation` 更新为新的路径位置。再次，由于路径中的每个偏移量都与网格单元的左上角相关，我们向目标位置添加 `16,16` 以使其居中。如果没有更多步骤，我们停止动画和声音。

在每个步骤更改时，我们调整角色面朝的方向，以确保角色面朝前往下一个路径步骤的方向。

- 11. 现在我们已经定义了三个移动函数，让我们重写简化的 `update` 函数。用以下使用我们新移动函数的新代码完全替换当前的 `update` 函数：

