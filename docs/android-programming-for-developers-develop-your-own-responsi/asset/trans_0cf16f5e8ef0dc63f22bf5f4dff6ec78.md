

## 编写 update 方法

update 方法相当长，因此我们将分段逐步编写，以确保所有内容都得到解释。

添加 update 方法的签名和方法体，我们将逐步将所有代码添加到其中：

```
// Everything that needs to be updated goes in here
// Movement, collision detection etc.
public void update(){
}
```

对我们的球和球拍调用 update 方法以处理任何所需的移动。将我们刚刚讨论的高亮代码添加到 update 方法中：

```
// Everything that needs to be updated goes in here
// Movement, collision detection etc.
public void update() {
    // Move the mPaddle if required
    mPaddle.update(mFPS);
    mBall.update(mFPS);
}
```

现在球和球拍已处于本帧的新位置，我们可以运行一系列测试，查看游戏中是否发生了任何重要情况。

第一个测试是查看球是否击中了球拍。使用球和球拍的 getRect 方法，我们将两个返回结果传递给 RectF 的静态 intersects 方法。如果球和球拍重叠（已接触），intersects 方法返回 true。

如果检测到碰撞，执行将进入 if 块并执行多项操作：

- 对球调用 setRandomXVelocity 以选择球返回屏幕上方时的随机水平方向
- 调用 reverseYVelocity 以开始返回屏幕上方
- 调用 clearObstacle，将球跳开几个像素，避免球卡在球拍上的可能性
- 递增 mScore 以增加玩家得分
- 从 SoundPool 播放一声提示音

将我们刚刚讨论的代码添加到 update 方法中：

```
// Check for mBall colliding with mPaddle
if(RectF.intersects(mPaddle.getRect(), mBall.getRect())) {
    mBall.setRandomXVelocity();
    mBall.reverseYVelocity();
    mBall.clearObstacleY(mPaddle.getRect().top - 2);

    mScore++;
    mBall.increaseVelocity();

    sp.play(beepID, 1, 1, 0, 0, 1);
}
...
```

接下来，我们处理球击中屏幕底部时的情况。检测此情况的方法是计算球底部的位置（mBall.getRect().bottom），并将其与屏幕的像素高度（mScreenY）进行比较。

如果发生了碰撞，则 if 块内会发生以下步骤：

- 反转球的速度
- 跳开几个像素，以防球卡住
- 递减 mLives
- 播放一段沉闷的声音
- 检查是否是最后一条生命，如果是，暂停游戏并调用 setupAndRestart

将我们刚刚讨论的高亮代码添加到 update 方法中：

```
// Bounce the mBall back when it hits the bottom of screen
if(mBall.getRect().bottom > mScreenY){
    mBall.reverseYVelocity();
    mBall.clearObstacleY(mScreenY - 2);

    // Lose a life
    mLives--;
    sp.play(loseLifeID, 1, 1, 0, 0, 1);

    if(mLives == 0){
        mPaused = true;
        setupAndRestart();
    }
}
```

接下来的代码使用球的顶部并将其与零进行比较，以查看是否已到达屏幕顶部。如果是，它只需在 y 轴上反转球，清除任何潜在障碍，并播放一声提示音。

将我们刚刚讨论的高亮代码添加到 update 方法中：

```
// Bounce the mBall back when it hits the top of screen
if(mBall.getRect().top < 0){
    mBall.reverseYVelocity();
    mBall.clearObstacleY(12);

    sp.play(beep2ID, 1, 1, 0, 0, 1);
}
```

接下来的代码使用球的左侧并将其与零进行比较，以查看是否已到达屏幕左侧。如果是，它只需在 x 轴上反转球，清除任何潜在障碍，并播放一声提示音。

将我们刚刚讨论的高亮代码添加到 update 方法中：

```
// If the mBall hits left wall bounce
if(mBall.getRect().left < 0){
    mBall.reverseXVelocity();
    mBall.clearObstacleX(2);

    sp.play(beep3ID, 1, 1, 0, 0, 1);
}
```

接下来的代码使用球的右侧并将其与 mScreenX 进行比较，以查看是否已到达屏幕右侧。如果是，它只需在 x 轴上反转球，清除任何潜在障碍，并播放一声提示音。

将我们刚刚讨论的高亮代码添加到 update 方法中：

```
// If the mBall hits right wall bounce
if(mBall.getRect().right > mScreenX){
    mBall.reverseXVelocity();
    mBall.clearObstacleX(mScreenX - 2);

    sp.play(beep3ID, 1, 1, 0, 0, 1);
}
```

现在，我们进入 draw 方法。

## 编写 draw 方法

我们首先要做的是尝试获取可绘图表面的锁并检查其是否有效。可以通过以下代码行实现（暂时不要添加它）：

```
if (mOurHolder.getSurface().isValid()) {

// Draw everything here

}
```

如果测试返回 true，我们几乎准备好进行绘制了。在开始使用画布之前，我们只需要这段代码（暂时不要添加）：

```
// Lock the mCanvas ready to draw
mCanvas = mOurHolder.lockCanvas();
```

现在，我们可以尽情使用 mPaint 了。按以下顺序，我们：

- 1. 确保表面有效并锁定画布，如刚才讨论的那样
- 2. 绘制背景
- 3. 更改画笔颜色
- 4. 通过传入 getRect 作为参数，将球拍绘制为矩形
- 5. 调用 getRect 作为参数，将球绘制为矩形
- 6. 再次更改画笔颜色
- 7. 更改文本大小
- 8. 在屏幕上绘制得分和生命数量
- 9. 调用 mOurHolder.unlockCanvasAndPost(mCanvas) 以完成本帧的绘制过程

将我们刚刚讨论的 draw 方法添加到 GameView 类中：

```
public void draw() {
// Make sure our drawing surface is valid or we crash
if (mOurHolder.getSurface().isValid()) {
// Draw everything here
// Lock the mCanvas ready to draw
mCanvas = mOurHolder.lockCanvas();
// Draw the background color
mCanvas.drawColor(Color.argb(255, 26, 128, 182));
// Choose the brush color for drawing
mPaint.setColor(Color.argb(255, 255, 255, 255));
// Draw the mPaddle
mCanvas.drawRect(mPaddle.getRect(), mPaint);
// Draw the mBall
mCanvas.drawRect(mBall.getRect(), mPaint);
// Choose the brush color for drawing
mPaint.setColor(Color.argb(255, 255, 255, 255));
// Draw the mScore
mPaint.setTextSize(40);
mCanvas.drawText("Score: " + mScore + "    Lives: " + mLives,
10, 50, mPaint);
// Draw everything to the screen
mOurHolder.unlockCanvasAndPost(mCanvas);
}
}
```

现在，我们可以实现 pause 和 resume 方法，它们停止和启动线程。当然，这些方法由 MainActivity 类在响应 Activity 生命周期方法时调用：

```
public void pause() {
mPlaying = false;
try {
mGameThread.join();
} catch (InterruptedException e) {
    Log.e("Error:", "joining thread");
}
}

// If the Activity starts/restarts
// start our thread.
public void resume() {
    mPlaying = true;
    mGameThread = new Thread(this);
    mGameThread.start();
}
```

我们乒乓球游戏的最后一大部分是处理用户触摸。为了让控制尽可能简单，我们规定：按住右侧任意位置会将球拍向右移动，按住左侧任意位置会将球拍向左移动。

当调用 onTouchEvent 方法时，我们根据事件类型进行切换。我们处理的第一个 case 是 MotionEvent.ACTION_DOWN。当玩家触摸屏幕时会发生这种情况。我们可以使用 motionEvent.getX 方法访问精确位置。

因此，在以下代码中，我们使用如下 if 语句：

```
if(motionEvent.getX() > mScreenX / 2){
```

这将确定是否在屏幕宽度一半以上的位置（即右侧）触摸了屏幕。如果上述语句为真，我们只需调用 paddle.setMovementState(mPaddle.RIGHT)，Paddle 类将在下次调用 update 时负责正确移动。

如果之前的 if 语句为假，则一定是在左侧触摸了屏幕，我们调用 paddle.setMovementState(mPaddle.LEFT)。

我们还需要记住，如果玩家从屏幕上移开手指，应停止球拍。我们可以在 switch 块的 MotionEvent.ACTION_UP case 中处理这一点。

添加重写的 onTouchEvent 方法：

