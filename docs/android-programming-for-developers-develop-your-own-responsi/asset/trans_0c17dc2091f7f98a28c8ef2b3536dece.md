

## 更新 GameEngine

从 GameEngine 的角度来看，使用 GameView 会带来一些影响。
这意味着它必须初始化 GameView，然后使用通用接口触发绘制。

GameView 将作为 GameEngine 构造函数的参数。它将通过传递游戏对象列表的引用来进行初始化。更新后的 GameEngine 构造函数如下：

```
public GameEngine (Activity a, GameView gameView) {
mActivity = a;
mGameView = gameView;
mGameView.setGameObjects(mGameObjects);

mWidth = gameView.getWidth()
- gameView.getPaddingRight() - gameView.getPaddingLeft();
mHeight = gameView.getHeight()
- gameView.getPaddingTop() - gameView.getPaddingBottom();

mPixelFactor = mHeight / 400d;
}
```

从现在起，我们还将在 GameEngine 内部计算 pixelFactor。我们将把它存储在一个公共变量中，以便游戏对象可以读取。这有几个好处，例如：

- 如果我们决定更改屏幕的单位数量，只需在一个地方进行更改
- 消除代码重复始终有利于维护

另一方面，GameEngine 的 onDraw 方法变得非常简单：

```
public void onDraw() {
mGameView.draw();
}
```

## 更新游戏布局

当然，我们必须修改 fragment_game.xml 布局以包含 GameView。我们将趁此机会对其进行其他修改，例如删除 TextView 并将布局的内边距改为暂停按钮的外边距。这样可以确保 GameView 占据全屏，同时保持按钮外边距不变。

重要的是要记住，在 FrameLayout 中，XML 中的顺序指定了项目的绘制顺序（z-index）。我们将把 GameView 放在布局的开头，以便暂停按钮绘制在其之上。

fragment_game.xml 的新版本如下：

```xml
<FrameLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:orientation="vertical"
tools:context="com.example.yass.counter.GameFragment">

<com.example.yass.engine.SurfaceGameView
android:id="@+id/gameView"
android:layout_width="match_parent"
android:layout_height="wrap_content" >

<Button
android:layout_gravity="top|right"
android:id="@+id/btn_play_pause"
android:layout_marginTop="@dimen/activity_vertical_margin"
android:layout_marginRight="@dimen/activity_vertical_margin"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="@string/pause" >

<include layout="@layout/view_vjoystick" />

</FrameLayout>
```

请注意，这是我们决定使用哪个 GameView 变体的地方。其余代码将通过 GameView 接口访问方法，因此不需要更改其他内容。从现在起我们将使用 SurfaceGameView，但也欢迎尝试 StandardGameView。

### 注意

布局是我们设置将使用哪个 GameView 变体的地方。

最后，在 GameFragment 内部，我们通过添加 GameView 参数来更新 GameEngine 的创建：

```
GameView gameView = (GameView)
getView().findViewById(R.id.gameView);
mGameEngine = new GameEngine(getActivity(), gameView);
```

现在我们有一个依赖 GameView 进行渲染的 GameEngine。我们仍然需要更新 GameObject 类以使用它。

在介绍 GameObject 类之前，让我们也花点时间改进 DrawThread。

## 改进 DrawThread

为了触发绘制，我们一直在使用计划好的 `Timer` 和 `TimerTask` 以达到每秒 30 帧。虽然这样可行，但像 `UpdateThread` 那样操作性能更好：尽可能多地调用 `onDraw`。

这种方法对 `SurfaceView` 非常有效，因为绘制是在同一线程上完成的。但使用 `StandardGameView` 时可能会出现消息溢出问题，因为它只是调用 `postInvalidate`。为了防止溢出，我们将确保两次 `onDraw` 调用之间的时间间隔永远不短于 20 毫秒，这足以支持每秒 50 帧。

新 `DrawThread` 的代码与 `UpdateThread` 完全相同，除了 run 方法中处理溢出的部分。它如下所示：

```
@Override
public void run() {
long elapsedMillis;
long currentTimeMillis;
long previousTimeMillis = System.currentTimeMillis();

while (mGameIsRunning) {
currentTimeMillis = System.currentTimeMillis();
elapsedMillis = currentTimeMillis - previousTimeMillis;
if (mPauseGame) {
while (mPauseGame) {
try {
synchronized (mLock) {
mLock.wait();
}
} catch (InterruptedException e) {
// We stay on the loop
}
}
}
currentTimeMillis = System.currentTimeMillis();
}
if (elapsedMillis < 20) { // This is 50 fps
try {
Thread.sleep(20-elapsedMillis);
} catch (InterruptedException e) {
// We just continue.
}
}
mGameEngine.onDraw();
previousTimeMillis = currentTimeMillis;
}
}
```

如果两次调用间隔太近，我们会让线程休眠剩余时间，最短为 20 毫秒。

如果发生 InterruptedException，我们其实没什么可处理的。因此，我们可以直接继续并调用 GameEngine 中的 onDraw。

## 精灵

我们已经提到过精灵的概念，精灵是在屏幕特定位置上绘制和处理的实体。本质上，游戏中我们看到的一切都是精灵。也有例外，例如游戏控制器（不绘制任何内容）和背景（以不同方式绘制），但我们将在本章后面讨论它们。

因此，这是继承自 GameObject 的 Sprite 类的代码：

```
public abstract class Sprite extends GameObject {
protected double mPositionX;
protected double mPositionY;
protected final double mPixelFactor;
private final Bitmap mBitmap;
protected final int mImageHeight;
protected final int mImageWidth;
private final Matrix mMatrix = new Matrix();
protected Sprite (GameEngine gameEngine, int drawableRes) {
Resources r = gameEngine.getContext().getResources();
Drawable spriteDrawable = r.getDrawable(drawableRes);
mPixelFactor = gameEngine.mPixelFactor;
mImageHeight = (int)
(spriteDrawable.getIntrinsicHeight()*mPixelFactor);
mImageWidth = (int)
(spriteDrawable.getIntrinsicWidth()*mPixelFactor);
mBitmap = ((BitmapDrawable) spriteDrawable).getBitmap();
}
@Override
public void onDraw(Canvas canvas) {
mMatrix.reset();
mMatrix.postScale((float) mPixelFactor, (float) mPixelFactor);
mMatrix.postTranslate((float) mPositionX, (float) mPositionY);
canvas.drawBitmap(mBitmap, mMatrix, null);
}
}
```

Sprite 是一个抽象类，根本没有实现 onUpdate。精灵关心的是在屏幕上显示一个项目，而不是该项目如何移动。

该类有一系列成员变量。让我们逐一介绍：

- mPositionX, mPositionY：Sprite 在屏幕上的位置。与我们用于 Player 和 Bullet 对象的概念相同。此位置位于图像的左上角。
- mPixelFactor：与之前相同的概念。将屏幕单位转换为像素的因子。
- mBitmap：我们要绘制的位图。
- mImageWidth, mImageHeight：位图在屏幕上绘制时的大小。此处设置是为了方便。我们始终可以使用位图和像素因子来计算它，但直接存储它以供子类将来使用更快。
- mMatrix：此对象是一个变换矩阵。它在将位图渲染到画布之前用于缩放、移动和旋转位图。出于优化目的，它在 onDraw 执行之间重复使用，而不是为每次运行创建。

下面是构造函数。我们传递 GameEngine 的引用和一个可绘制资源。在上一章中，我们传递父 View，以便可以将新创建的 ImageView 添加到其中。现在不再需要这样做了。我们只需要上下文和像素因子，可以从 GameEngine 获取。

构造函数中的代码与我们之前见过的非常相似。它执行多个操作：

- 通过上下文从资源加载 Drawable
- 获取可绘制对象的固有大小，并将其乘以像素因子，以存储精灵在特定设备上将使用的宽度和高度（像素）
- 从 Drawable 获取 Bitmap 并将其存储在类变量中，以便在 onDraw 期间使用

最后，我们有接收 Canvas 的 onDraw 方法。此方法现在由 GameView 调用。Canvas 的获取方式与从 a

