
### 小心浮点数

在进行计算时，整数运算大约比浮点运算快两倍。

当整数不够用时，float 和 double 在速度上没有实际差异。唯一的区别在于空间，double 占用空间是 float 的两倍。

此外，即使是整数，某些处理器也有硬件乘法，但缺乏硬件除法。在这种情况下，整数除法和取模操作是在软件中执行的。总之，这是过早优化可能会伤害你的一个案例。

### 性能迷思 – 避免使用接口

在较旧版本的 Android 中，在引入 JIT 编译器之前，
通过接口而不是确切类型来访问方法会略微更高效。在这些版本中，声明 ArrayList 类型的变量而不是通用 List 接口以直接访问类是有意义的。

然而，在现代版本的 Android 中，通过接口访问变量与直接访问没有区别。因此，为了通用性，我们将使用通用接口而不是类，如 GameEngine 中所示：

```
private List<GameObject> mGameObjects = new ArrayList<GameObject>();
```

## 摘要

在简要介绍了哪些工具最适合制作哪种类型的游戏之后，我们已经描述了使用原生 Android SDK 制作游戏的优缺点。

我们已经设置了一个项目并定义了主活动及其方向。我们已经创建了一个基本游戏引擎，将其包含在项目中，并检查了它是否按预期工作。

后来，我们通过第二个碎片和暂停对话框扩展了项目，正确管理了游戏的生命周期，并定义了一种针对不同 Android 版本获得合适全屏的方法。

最后，我们介绍了一些关于优化游戏关键部分代码的技巧。

我们准备开始处理用户输入。

## 第 2 章. 管理用户输入

在本章中，我们将学习如何以通用的方式处理用户输入，稍后将其扩展为虚拟摇杆、传感器或外部控制器。

为了获得输入的可视反馈，我们将在屏幕上放置一艘飞船并将其移动。我们还将使它发射一些子弹。这也将帮助你理解游戏对象和游戏引擎之间的交互。

我们将扩展通用 InputController 类来制作最简单的键盘控制器，以了解该类如何融入现有架构以及输入如何在游戏对象中被处理和读取。

一旦基本键盘工作正常，我们将实现一个虚拟摇杆，这是一种处理用户输入的更好的方式。

管理物理控制器对于我们正在编写的游戏也很重要，因此我们将了解如何检测它们并处理不同选项。

最后，我们将简要谈谈将传感器作为控件的使用。它们不适合这种类型的游戏，但我们将介绍基础知识，如果你想进一步探索，我们还会提供一些链接。

### InputController 基类

无论我们想如何控制游戏，它都可以始终抽象为一个双轴摇杆和一个发射按钮。对于其他游戏可能不同，但你应该始终能够从用户那里提取基本操作并创建一个处理它们的 InputController。我们正在构建的 InputController 适用于任何使用方向控制的游戏。

### 注意

输入控制器对于每种类型的游戏都相当特定。

我们将把归一化的水平轴和垂直轴视为输入（范围从 -1 到 1）。在控制器没有范围的情况下，我们将把它设置为最大值。当输入类型允许时，这将允许我们精确处理用户输入，就像虚拟和真实摇杆以及传感器的情况一样。

![](img/35d01831c19688a44e50bce7c489007a_1257_0.png)

*手机屏幕的坐标系统*

提醒一下，计算机屏幕上的坐标在左上角为 [0,0]，向右和向下为正。右下角的坐标为 [width, height]。这与我们习惯的标准坐标系不同，但请务必记住这一点。

这就是为什么向左移动是 -1，向上移动也是 -1。

我们游戏中所有输入控制器的基类将如下所示：

```
public class InputController {
    public double mHorizontalFactor;
    public double mVerticalFactor;

    public boolean mIsFiring;

    public void onStart() {
    }

    public void onStop() {
    }

    public void onPause() {
    }

    public void onResume() {
    }
}
```

请注意，这是一个带有公共变量的类。这样做是为了避免通过方法读取值。我们在上一章中将其作为性能改进提及过。

这个类的每个实现都负责用更新后的值填充这些变量。游戏对象可以在 onUpdate 期间读取它们。通过这样做，我们将使用值的操作与读取用户输入分开。

### 注意

InputController 将输入的读取与通过游戏对象的使用隔离开来。

InputController 是 GameEngine 的一部分。我们只需在引擎中添加此类型的变量并创建一个方法来设置它：

```
public InputController mInputController;

public void setInputController(InputController controller) {
    mInputController = controller;
}
```

onStart、onStop、onPause 和 onResume 方法由 GameEngine 在游戏启动、停止、暂停或恢复时调用。某些输入控制器在这些情况下需要执行特殊操作。

最后，我们在 GameFragment 内部初始化引擎期间将输入控制器添加到 GameEngine 中：

```
mGameEngine = new GameEngine(getActivity());
mGameEngine.addGameObject(new ScoreGameObject(view,
R.id.txt_score));
view.findViewById(R.id.btn_play_pause).setOnClickListener(this);
mGameEngine.setInputController(new InputController());
mGameEngine.addGameObject(new Player(getView()));
mGameEngine.startGame();
```

目前，我们添加了一个不执行任何操作的输入控制器。我们还添加了一个 Player 游戏对象，我们将在深入研究不同的输入控制器之前先处理它。

请注意，我们不再使用之前示例中的 ScoreGameObject，不应将其添加到游戏引擎中。

### Player 对象

我们将构建的第一个版本的 Player 游戏对象将只在屏幕中间初始化其坐标。然后，它将根据输入控制器中的信息更新它们，最后，它将在布局中的 TextView 中以 [x, y] 形式显示值。

之后，我们将使它显示位于坐标处的飞船。但目前，我们将专注于 onUpdate 的实现方式。

Player 类第一个版本的 onUpdate 和 onDraw 的代码如下所示：

```
@Override
public void onUpdate(long elapsedMillis, GameEngine gameEngine) {
    InputController inputController = gameEngine.inputController;
    mPositionX += mSpeedFactor*inputController.mHorizontalFactor*elapsedMillis;
    if (mPositionX < 0) {
    mPositionX = 0;
    }
    if (mPositionX > mMaxX) {
    mPositionX = mMaxX;
    }
    mPositionY += mSpeedFactor*inputController.mVerticalFactor*elapsedMillis;
    if (mPositionY < 0) {
    mPositionY = 0;
    }
    if (mPositionY > mMaxY) {
    mPositionY = mMaxY;
    }
}

@Override
public void onDraw() {
    mTextView.setText("["+(int) (mPositionX)+","+(int) (mPositionY)+"]");
}
```

因此，在每次 onUpdate 运行中，我们将使用相应的系数（我们从输入控制器中读取）、速度系数和经过的毫秒数来增加 x 和 y 位置。这无非是经典的公式
