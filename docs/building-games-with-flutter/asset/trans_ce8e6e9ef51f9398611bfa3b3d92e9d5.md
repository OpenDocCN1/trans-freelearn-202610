

将 mixin 添加到 `Background` 类并覆盖 `onTapUp` 以获取可用于计算移动的 `x` 和 `y` 位置。

因为我们需要知道 `George` 类内部的这个坐标，所以需要将 `George` 类的引用传入 `Background` 类，以便在玩家触摸屏幕时传递点击事件。

要开始，让我们修改 `Background` 类：

- 1. 打开 `background.dart` 文件。在类顶部添加以下构造函数和变量，用于保存对 `George` 类的引用。我们将使用它在玩家触摸屏幕时将事件传回 `George` 类，并调用 `George` 类上的名为 `moveToLocation` 的函数，传递点击事件：

```
Background(this.george);
final George george;
```

- 2. 使用以下 import 解析对 `George` 类的引用：

```
import 'george.dart';
```

- 3. 通过添加 `Tappable` mixin 更改 `Background` 类定义：

```
class Background extends PositionComponent with Tappable {
```

- 4. 在类底部添加以下代码以覆盖 `onTapUp` 函数，并将 `TapUpInfo` 事件传递到 `George` 类：

```
@override
bool onTapUp(TapUpInfo info) {
  george.moveToLocation(info);
  return true;
}
```

目前，`moveToLocation` 函数在 `George` 类中不存在，但我们将在更新 `George` 类时稍后添加它。因为我们将使用触摸事件，所以从该函数返回 `true` 以指示触摸事件已被处理。

- 5. 添加以下 import 以解析 `TapUpInfo` 引用：

```
import 'package:flame/input.dart';
```

- 6. 在 `main.dart` 文件的 `onLoad` 函数中，让我们将 `George` 类变成一个变量，然后我们可以将其传递给 `Background` 类构造函数。为此，请将 `onLoad` 函数的前几行更改如下：

```
@override
Future<void> onLoad() async {
  var hud = HudComponent();
  var george = George(hud: hud, position:
    Vector2(200, 400), size: Vector2(48.0, 48.0),
    speed: 40.0);
  add(Background(george));
  add (george);
}
```

请注意，这里只展示了 `onLoad` 函数的顶部部分以显示更改的行，但添加敌人、HUD 和 `ScreenCollidable` 的其余行保持不变。

在对 `George` 类进行最终更改之前，让我们创建一个数学工具类来存储一个函数，用于获取 George 位置与玩家触摸屏幕位置之间的角度。

- 7. 在 `lib` 文件夹中，创建一个名为 `utils` 的新文件夹，并在该文件夹中创建一个名为 `math_utils.dart` 的新文件。

- 8. 打开新的 `math_utils.dart` 文件并为 `getAngle` 函数添加以下代码：

```
import 'dart:math';
import 'package:flame/components.dart';

double getAngle(Vector2 origin, Vector2 target) {
  double dx = target.x - origin.x;
  double dy = -(target.y - origin.y);
  double angleInRadians = atan2(dy, dx);
  if (angleInRadians < 0) {
    angleInRadians = angleInRadians.abs();
  } else {
    angleInRadians = 2 * pi - angleInRadians;
  }
  return angleInRadians * radians2Degrees;
}
```

我们不会详细介绍 `getAngle` 函数的工作原理，但它使用了大多数高中数学课中涵盖的基本三角学。`getAngle` 函数将返回 0 到 360 之间的角度，0 度朝向右侧，90 度朝向下方，180 度朝向左侧，270 度朝向上方。

- 9. 打开 `george.dart` 文件并为数学工具类添加以下 imports：

```
import 'package:goldrush/utils/math_utils.dart';
```

- 10. 在 `George` 类中，在构造函数之后，让我们添加几个用于 `targetLocation` 向量的变量以及一个用于跟踪是否通过触摸移动的变量：

```
late Vector2 targetLocation;
bool movingToTouchedLocation = false;
```

- 11. 现在，让我们在 `Background` 组件中添加前面引用的 `moveToLocation` 函数，将 `targetLocation` 设置为传递的触摸事件，并将 `movingToTouchedLocation` 设置为 true：

```
void moveToLocation(TapUpInfo info) {
    targetLocation = info.eventPosition.game;
    movingToTouchedLocation = true;
}
```

精灵具有一个默认在 Flame 中位于精灵左上角的锚点，因此如果我们在屏幕上触摸，精灵将被移动，并且精灵的左上角将与屏幕上的触摸点对齐。将精灵围绕其中心而不是左上角居中会更自然，因此让我们将锚点更新为中心。

- 12. 在 `onLoad` 函数底部且在添加命中框之前，添加以下代码以将锚点设置在精灵中心。

```
anchor = Anchor.center;
addHitbox(HitboxRectangle());
```

- 13. 最后，我们将向 `update` 函数添加代码，将 George 移动到触摸位置。在 `update` 函数中，查找包含以下代码的 `else` 块：

```
if (playing) {
stopAnimations();
}
```

将其替换为以下代码块：

```
if (movingToTouchedLocation) {
} else {
if (playing) {
stopAnimations();
}
}
```

- 14. 在 `if` 代码块内部，添加以下代码以设置新的锚点位置：

```
position += (targetLocation - position).normalized()
* (speed * dt);
```

此代码根据 `targetLocation` 与当前 `position` 之间的差异更新 George 的位置，同时考虑当前 `speed` 和自上次调用更新以来的时间，以确保移动平滑。

- 15. 当我们到达触摸位置时，我们希望停止任何正在播放的动画并将 `movingToTouchedLocation` 设置为 `false`。

因为我们基于可能存在舍入误差的分数来移动角色，所以我们允许一个小阈值来决定 George 是否足够接近触摸位置，如果是，我们停止动画。所以，让我们继续在这个代码块内部并添加阈值检查：

```
double threshold = 1.0;
var difference = targetLocation - position;
if (difference.x.abs() < threshold &&
    difference.y.abs() < threshold) {
    stopAnimations();
    movingToTouchedLocation = false;
    return;
}
```

- 16. 最后，我们将使用之前创建的 getAngle 函数，并使用角度来决定 George 正在朝哪个方向移动以及哪个动画最接近。例如，我们只有四个动画 – 上、下、左、右 – 因此我们将通过将方向分成四个 90 度象限来近似方向，如下图所示：

![](img/a63c3fccbe888cd4d04006b5e1b6c273_98_0.png)

图 5.2 – 移动方向的象限映射

根据我们的图表，如果我们在 45 度到 135 度之间移动，我们大约在向下移动，因此可以将方向设置为下并使用 downAnimation 正确地为精灵设置动画。请记住，0 度指向东，我们需要两个检查来向右移动，即 315 到 360 度和 0 到 45 度之间。所以，让我们为这个逻辑添加代码，继续在同一代码块中：

```
playing = true;
var angle = getAngle(position, targetLocation);
if ((angle > 315 && angle < 360) || (angle > 0 && angle < 45)) { // Moving right
animation = rightAnimation;
currentDirection = Character.right;
}
// Moving down
else if (angle > 45 && angle < 135) {
animation = downAnimation;
currentDirection = Character.down;
}
// Moving left
else if (angle > 135 && angle < 225) {
animation = leftAnimation;
currentDirection = Character.left;
}
// Moving up
else if (angle > 225 && angle < 315) {
animation = upAnimation;
currentDirection = Character.up;
}
```

现在，如果你运行代码，可以通过触摸屏幕上的任何位置来控制 George，他会移动到那里。如果你愿意，也可以使用操纵杆。无论你更喜欢哪种移动方式，按住运行按钮仍然会让 George 跑得更快。还要注意，如果你使用触摸来移动角色，然后切换到操纵杆控制，它将中断由触摸引起的移动，允许你手动控制 George 的移动。

## 摘要

本章涵盖了大量内容，游戏真的开始成形了。我们学习了如何使用操纵杆和运行按钮为游戏创建屏幕控件，以及如何在屏幕上添加分数文本。然后，我们学习了一种替代方法来控制 George，即允许你触摸屏幕将他移动到触摸位置。

不过，游戏目前有点安静。在大多数游戏中，音乐配乐和音效用于营造氛围并使游戏生动起来。在下一章中，我们将为游戏添加一些音乐和声音以进一步增强它。

