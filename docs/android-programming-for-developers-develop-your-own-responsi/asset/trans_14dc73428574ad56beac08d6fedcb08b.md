

## 物理控制器

是时候进入硬核玩家喜爱的一种控制器类型了：物理控制器。

有一些设备将控制器作为硬件的一部分包含在内。一些著名的例子是 Xperia Play——最早带有滑动游戏手柄的手机之一——以及 Nvidia Shield，该类别中的最新设备。

Xperia Play 是最早配备集成游戏手柄的设备之一：

![](img/35d01831c19688a44e50bce7c489007a_1291_0.png)

Nvidia Shield 是配备游戏手柄的最强大的 Android 设备之一：

![](img/35d01831c19688a44e50bce7c489007a_1292_0.png)

另一方面，许多品牌为智能手机制造游戏控制器，并在传统玩家中相当流行。它们都是可以连接到您手机或平板电脑的蓝牙控制器。其中一些设计为让手机插入其中，例如 Gametel（另一款先驱）和大多数 MOGA 型号。

![](img/35d01831c19688a44e50bce7c489007a_1293_0.png)

*带有可调节条带以固定手机的 MOGA 控制器*

还有少数 Android 驱动的设备将控制器作为主要输入源。这里我们说的是微型主机，例如 OUYA，或其他类似电视的设备，例如 Amazon FireTV 或 Android TV。

![](img/35d01831c19688a44e50bce7c489007a_1294_0.png)

*OUYA 是第一款 Android 微型主机*

这些设备的工作方式与 HID 设备非常相似，无论是以键盘的形式，还是作为基于轴的方向控制（模拟摇杆）。我们要做的只是设置正确的监听器。

有些控制器确实有自己的专有库。我们不会涉及此内容，因为它们非常具体，并提供了有关如何集成它们的详细文档。MOGA Pocket 就是这种情况（更高级的 MOGA 控制器支持两种模式：专有和 HID）。

## 注意

大多数控制器都作为 HID 工作，这是标准。

我们可以在 Activity 级别或视图级别为控制器设置监听器。无论如何，我们都需要扩展该类。无法为这些方法添加监听器，必须重写它们。由于我们已经扩展了 Activity 类，我们将这样做。

## 注意

我们在 Activity 内部监听 KeyEvent 和 MotionEvent。

我们需要监听两种类型的事件。如下所示：

- KeyEvent：用于所有按钮按下，在某些游戏手柄中，也用于方向十字键
- MotionEvent：与沿轴移动相关的事件：摇杆

我们希望将输入控制器与 Activity 分离，因此将创建一个特殊的监听器来组合我们需要的两个事件，然后让 Activity 委托给它。

我们需要的接口非常简单，我们将其命名为 GamepadControllerListener：

```java
public interface GamepadControllerListener {
    boolean dispatchGenericMotionEvent(MotionEvent event);
    boolean dispatchKeyEvent(KeyEvent event);
}
```

在 Activity 内部，我们创建一个方法来设置 GamepadControllerListener 类型的监听器。由于我们一次只需要一个监听器，因此该方法是 set 而不是 add。要移除监听器，只需将其设置为 null：

```java
public void setGamepadControllerListener(GamepadControllerListener listener) {
    mGamepadControllerListener = listener;
}
```

最后，我们必须在 Activity 中重写 dispatchGenericMotionEvent 和 dispatchKeyEvent：

```java
@Override
public boolean dispatchGenericMotionEvent(MotionEvent event) {
    if (mGamepadControllerListener != null) {
        if (mGamepadControllerListener.dispatchGenericMotionEvent(event)) {
            return true;
        }
    }
    return super.dispatchGenericMotionEvent(event);
}

@Override
public boolean dispatchKeyEvent(KeyEvent event) {
    if (mGamepadControllerListener != null) {
        if (mGamepadControllerListener.dispatchKeyEvent(event)) {
            return true;
        }
    }
    return super.dispatchKeyEvent(event);
}
```

请注意，此方法使用约定：如果事件已使用则返回 true，如果未使用则返回 false。在我们的情况中，只有当事件由监听器使用时才会返回 true。在其他情况下，我们将返回结果以将事件委托给基类。

调用超类中的相应方法非常重要，因为 Activity 类内部有大量我们不希望意外丢弃的处理。

准备好这些组件后，我们可以继续创建 GamepadInputController，它将扩展 InputController 并实现 GamepadControllerListener：

```java
public class GamepadInputController
        extends InputController
        implements GamepadControllerListener {

    public GamepadInputController(YassActivity activity) {
        mActivity = activity;
    }

    @Override
    public void onStart() {
        mActivity.setGamepadControllerListener(this);
    }

    @Override
    public void onStop() {
        mActivity.setGamepadControllerListener(null);
    }
}
```

```
[...]
}
```

尽可能限制对 Activity 的键盘和运动事件的挂钩是我们想要做到的。这就是为什么我们重写 InputController 的 onStop 和 onStart 方法，以便仅在游戏运行时设置监听器。

有几种可能的控制器布局。通常，它们都有十字控制和/或模拟摇杆。有些有多个模拟摇杆，当然还有按钮。总的来说，关于事件以及不同游戏手柄的配置方式，有一些重要细节：

![](img/35d01831c19688a44e50bce7c489007a_1297_0.png)

- 十字控制可以由按钮组成，也可以是另一个模拟摇杆。如果它有按钮，则将其作为与 D-Pad 相同常量的 KeyEvent 处理。如果它是模拟摇杆，则会使用 AXIS_HAT_X 和 AXIS_HAT_Y。

- 模拟摇杆通过 MotionEvent 处理，我们可以使用 MotionEvent 的 getAxisValue 方法读取它们。默认摇杆将使用 AXIS_X 和 AXIS_Y。

- 我们不会为这个游戏映射第二个模拟摇杆，但它映射在 AXIS_Z 和 AXIS_RZ 上。

- 按钮映射为带有每个按钮名称的 KeyEvent。

## 处理 MotionEvents

当我们收到 MotionEvent 时，首先需要验证该事件来自我们应该读取的来源。

来源是事件的一部分，它是标志的组合。我们感兴趣的有：

- SOURCE_GAMEPAD：表示设备具有游戏手柄按钮，例如 A、B、X 或 Y。
- SOURCE_DPAD：表示设备具有 D-Pad。
- SOURCE_JOYSTICK：表示设备具有模拟控制杆。

我们应该处理的唯一动作事件是源设置了摇杆标志的事件。游戏手柄和 D-Pad 源都将作为 KeyEvent 发送。

接收 MotionEvent 的处理方式如下：

```java
@Override
public boolean dispatchGenericMotionEvent(MotionEvent event) {
    int source = event.getSource();

    if ((source & InputDevice.SOURCE_JOYSTICK) != InputDevice.SOURCE_JOYSTICK) {
        return false;
    }
    mHorizontalFactor = event.getAxisValue(MotionEvent.AXIS_X);
    mVerticalFactor = event.getAxisValue(MotionEvent.AXIS_Y);

    InputDevice device = event.getDevice();
    MotionRange rangeX = device.getMotionRange(MotionEvent.AXIS_X, source);
    if (Math.abs(mHorizontalFactor) <= rangeX.getFlat()) {
        mHorizontalFactor = event.getAxisValue(MotionEvent.AXIS_HAT_X);
        MotionRange rangeHatX = device.getMotionRange(MotionEvent.AXIS_HAT_X, source);
        if (Math.abs(mHorizontalFactor) <= rangeHatX.getFlat()) {
            mHorizontalFactor = 0;
        }
    }
    MotionRange rangeY = device.getMotionRange(MotionEvent.AXIS_Y, source);
    if (Math.abs(mVerticalFactor) <= rangeY.getFlat()) {
        mVerticalFactor = event.getAxisValue(MotionEvent.AXIS_HAT_Y);
        MotionRange rangeHatY =
                device.getMotionRange(MotionEvent.AXIS_HAT_Y, source);
        if (Math.abs(mVerticalFactor) <= rangeHatY.getFlat()) {
            mVerticalFactor = 0;
        }
    }
    return true;
}
```

首先，我们检查源。如果它不是来自摇杆，我们就直接返回 false，因为我们不会使用这个事件。

然后我们读取 `MotionEvent.AXIS_X` 和 `MotionEvent.AXIS_Y` 的轴值，并将它们赋值给我们的变量。这旨在读取默认摇杆。但我们还没结束。控制器可能有一个充当模拟摇杆的十字键。

为了决定是否读取次级摇杆，我们检查默认摇杆上是否有输入。如果没有，我们将次级摇杆的值赋给我们的变量。

需要注意的是，大多数模拟摇杆并没有完美地对齐到 0，因此将 `mHorizontalFactor` 和 `mVerticalFactor` 的值与 0 进行比较不是检测摇杆是否移动的有效方法。

## 注意

模拟摇杆并没有完美地居中于 0。

我们需要做的是读取设备运动范围的平坦值。这比听起来要简单得多，因为所有这些信息都是 `MotionEvent` 的一部分。

然后，如果默认轴上没有输入，我们将 `AXIS_HAT_X` 和 `AXIS_HAT_Y` 的值赋给我们的变量。我们还检查轴输入是否高于其平坦值，如果不是，则将其设置为 0。我们需要这样做，否则飞船在没有任何输入的情况下也会非常缓慢地移动。

最后，我们返回 true 以指示我们已使用该事件。

