

在任何情况下，传感器都与设备的默认方向相关，该方向可以是横向或纵向。在处理数值时，我们必须考虑到这一点。

总而言之，水平轴的转换可以这样实现：

```
private double getHorizontalAxis() {
if (SensorManager.getRotationMatrix(mRotationMatrix, null,
mLastAccels, mLastMagFields)) {
if (mRotation == Surface.ROTATION_0) {
SensorManager.remapCoordinateSystem(mRotationMatrix,
SensorManager.AXIS_Y, SensorManager.AXIS_MINUS_X, mRotationMatrix);
SensorManager.getOrientation(mRotationMatrix, mOrientation);
return mOrientation[1] * DEGREES_PER_RADIAN;
}
else {
SensorManager.getOrientation(mRotationMatrix, mOrientation);
return -mOrientation[1] * DEGREES_PER_RADIAN;
}
}
else {
// Case for devices which do NOT have magnetic sensors
if (mRotation == Surface.ROTATION_0) {
return -mLastAccels[0] * 5;
}
else {
return -mLastAccels[1] * -5;
}
}
}
```

getHorizontalAxis 代码执行以下步骤：

- 使用加速度计和磁传感器的最后数据计算旋转矩阵。
- 如果返回 true，则一切正常。根据设备的旋转，我们决定是否需要重新映射坐标系，然后返回转换为角度的方向。
- 如果无法计算（缺少磁场传感器），该方法返回 false。我们必须依赖使用加速度计数值的近似值。根据设备的旋转，我们应该使用其中一个轴或另一个轴。

设备的旋转可以在 InputController 的构造函数中用一行代码读取。

```
mRotation =
yassActivity.getWindowManager().getDefaultDisplay().getRotation();
```

最后是 onPreUpdate 方法：

```
@Override
public void onPreUpdate() {
    mHorizontalFactor = getHorizontalAxis() / MAX_ANGLE;
    if (mHorizontalFactor > 1) {
        mHorizontalFactor = 1;
    }
    else if (mHorizontalFactor < -1) {
        mHorizontalFactor = -1;
    }
    mVerticalFactor = 0;
}
```

此方法只是使用我们认为设备完全倾斜时的最大角度，将读数（以度为单位）转换为 [-1,1] 范围内的值。我建议你尝试这个常量，并从 30 度开始。

有关传感器处理的更多信息，可以查看官方文档

http://developer.android.com/guide/topics/sensors/sensors_overview.html.

## 选择控制模式

游戏通常会要求用户选择他们偏好的控制模式，但避免不必要的询问、尽可能减少摩擦也是一种良好做法。

YASS 仅使用虚拟摇杆和游戏手柄控件。无需询问用户想要哪一种。两种输入模式都是兼容的，尤其是因为虚拟摇杆在未使用时不会在屏幕上显示任何内容。我们需要做的唯一事情是修改 GameEngine 以支持多个 InputController。

**注意**

我们将同时支持两种输入模式。

同时支持两种输入模式的方法是创建一个 CompositeInputController，它使用组合模式同时拥有一个 VirtualJoystickInputController 和一个 GamepadInputController，并合并两者的输入。

为了同步两个输入控制器的读数，我们将使用 InputController 上的一个名为 onPreUpdate 的方法，该方法将在 onUpdate 之前立即调用。我们将使用它，用其他控制器读取的值来填充 mHorizontalFactor、mVerticalFactor 和 mIsFiring 的值。

```
public void onPreUpdate() {
    mIsFiring = mGamepadInputController.mIsFiring ||
    mVJoystickInputController.mIsFiring;
    mHorizontalFactor = mGamepadInputController.mHorizontalFactor +
    mVJoystickInputController.mHorizontalFactor;
    mVerticalFactor = mGamepadInputController.mVerticalFactor +
    mVJoystickInputController.mVerticalFactor;
}
```

现在我们有了一个可以使用虚拟摇杆或游戏手柄控制的游戏。

## 总结

我们已经学习了几种处理用户输入的方法，以及如何使其对 GameEngine 透明。

为了从控制器获得适当的视觉反馈，我们创建了一个 Player 游戏对象，根据 InputController 的值更新其位置。我们还学习了如何在游戏过程中向 GameEngine 添加和移除游戏对象。

我们创建了一个非常基本的键盘，后来演变成了虚拟摇杆。我们还学习了如何处理外部控制器。

此时，我们的游戏中有一艘沿屏幕移动并发射子弹的飞船。它可以独立地使用虚拟摇杆或游戏手柄进行控制。

当前实现偶尔会出现卡顿，而且我们刚刚开始在屏幕上绘制对象。是时候解决这个问题了。下一步：通过直接在视图上绘制而不是依赖于在屏幕上定位视图来改进渲染。

## 第 3 章. 进入绘制线程

在本章中，我们将改进游戏中精灵的渲染。为此，我们将使用一个自定义 GameView 来执行低级绘制。我们将制作两种不同的实现：一种扩展自 View，另一种扩展自 SurfaceView。我们将让 DrawThread 成为一个真正的线程，以便更好地与这个 GameView 配合工作。

我们将重构项目，创建一个 Sprite 类，它将用于游戏中绘制的所有项目。我们将在 Canvas 上绘制位图，并学习用于此目的的转换矩阵。

为了继续改进游戏，我们将添加敌人。它们将是一波朝我们的飞船移动的小行星。为此，我们将学习 GameController 的概念以及不同的实现方式，从静态到程序化关卡生成。

作为渲染技术的一部分，我们将学习遮挡剔除和视差背景，我们将使用它们使游戏看起来更好。

最后，我们将为引擎添加图层支持。

## 使用 GameView

到目前为止，我们一直在使用标准视图并对其进行转换来渲染游戏的不同元素。虽然这是在屏幕上绘制元素的一种简单方法，但远非高效。我们依赖系统布局来进行绘制。

虽然这种技术对于回合制游戏或任何非实时游戏来说没问题，但它无法为实时游戏渲染足够的帧率。

> **注意**
> 使用标准 Views 对于非实时游戏来说没问题。

我们将创建一个名为 GameView 的自定义视图。该视图将负责绘制精灵。

我们之前已经注意到代码重复，并在上一章中提到了精灵的概念。现在我们将向前迈进，创建一个 Sprite 类，它将负责在 GameView 内的特定坐标处绘制图像。

在 Android 上进行低级绘制有两种方式：

- 扩展 View 并重写 onDraw
- 扩展 SurfaceView 并使用 SurfaceHolder

在这两种情况下，我们都会得到一个 Canvas 并在上面绘制我们的游戏对象。主要区别在于 View 的 onDraw 方法在 UIThread 上执行，而 SurfaceView 和 SurfaceHolder 旨在在单独的线程上执行绘制。

> **注意**
> 在 Android 上，低级绘制始终使用画布进行。

根据官方文档，使用 SurfaceView 更高效。但是，自 Android 4.0 以来，视图渲染是硬件加速的（而 SurfaceView 不是）。对于拥有高分辨率屏幕和更快处理器的现代手机来说，情况不一定总是如此。

> **注意**
> SurfaceView 不是硬件加速的，性能可能比普通 View 差。

无论如何，你应该两者都了解，并能轻松切换它们，即使只是为了测试目的。我们将创建一个名为 GameView 的接口，由这两个类实现，以便可以轻松更改。我们将创建的类是：

- StandardGameView：将扩展自 View
- SurfaceGameView：将扩展自 SurfaceView

