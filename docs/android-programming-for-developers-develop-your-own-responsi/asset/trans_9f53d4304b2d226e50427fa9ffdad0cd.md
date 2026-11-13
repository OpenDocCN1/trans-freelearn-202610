
让我们从对 GameFragment 的修改开始。这个类将实现 GameOverDialogListener。

巧合的是，GameFragment 已经有一个名为 exitGame 的方法，我们之前从 PauseDialog 调用它，因此只需实现 startNewGame。

为此，我们将创建 GameEngine 的所有逻辑提取到一个方法中，然后从游戏可以开始的两个位置调用它：

- 片段创建并测量时的 onGlobalLayout 方法
- 当在 GameOver 对话框中点击“再玩一次？”按钮时的 startNewGame

代码如下：

```
@Override
public void startNewGame() {
// Exit the current game
mGameEngine.stopGame();
// Start a new one
prepareAndStartGame();
}

private void prepareAndStartGame() {
GameView gameView = (GameView)
getView().findViewById(R.id.gameView);
mGameEngine = new GameEngine(getActivity(), gameView, 4);
mGameEngine.setInputController(new
CompositeInputController(getView(), getYassActivity()));
mGameEngine.setSoundManager(getYassActivity().getSoundManager());
new ParallaxBackground(mGameEngine, 20,
R.drawable.seamless__space_0).addToGameEngine(mGameEngine, 0);
new GameController(mGameEngine,
GameFragment.this).addToGameEngine(mGameEngine, 2);
new FPSCounter(mGameEngine).addToGameEngine(mGameEngine, 2);
new ScoreGameObject(getView(),
R.id.score_value).addToGameEngine(mGameEngine, 0);
new LivesCounter(getView(),
R.id.lives_value).addToGameEngine(mGameEngine, 0);
mGameEngine.startGame();
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
InputManager inputManager = (InputManager)
getActivity().getSystemService(Context.INPUT_SERVICE);
inputManager.registerInputDeviceListener(GameFragment.this,
null);
}
}
```

当我们开始游戏时，首先必须停止之前的游戏。即使 GameOverDialog 在前面，GameEngine 也不会暂停或停止。事实上，我们可以看到视差背景在对话框后面移动。这是一个很好的效果，我们希望保持这样，但这意味着在开始新游戏之前需要停止当前游戏，否则会有两个引擎同时运行，这对性能和电池寿命都很糟糕。

请注意，初始化中没有创建 Player。这是我们在让 GameController 管理生命时移除的内容，但再次强调这一点很有价值。

另一方面，我们需要能够从 GameController 内部显示对话框，因为显示对话框的实用方法是在基片段上实现的。这就是为什么我们需要向构造函数传递一个参数并将其存储为成员变量。

```
public GameController(GameEngine gameEngine, GameFragment parent) {
 mParent = parent;
 [...]
}
```

```
@Override
public void onGameEvent(GameEvent gameEvent) {
 [...]
 else if (gameEvent == GameEvent.GameOver) {
 mState = GameControllerState.GameOver;
 showGameOverDialog();
 }
}
```

```
private void showGameOverDialog() {
 mParent.getActivity().runOnUiThread(new Runnable() {
 @Override
 public void run() {
 GameOverDialog quitDialog = new GameOverDialog(mParent);
 quitDialog.setListener(mParent);
 mParent.showDialog(quitDialog);
 }
 });
}
```

作为对 GameOver 事件处理的一部分，我们创建并显示 GameOverDialog。请注意，由于游戏事件在 UpdateThread 中到达，而对话框的创建和显示必须在 UIThread 上运行，因此我们需要使用 Runnable 对象。

## 其他对话框

游戏中还有两个我们尚未使用的对话框。这些对话框是：

- 控制器已连接的通知对话框
- 触摸控件信息对话框

两个对话框只是一张图片，点击它们应该会关闭它们，因此没有复杂的布局，也不需要自定义监听器。

不过，我们将在第 7 章《投射到大屏幕》中处理控制器已连接对话框，因为我们希望在不同情况下显示它：在电视上而不是手机上。

## 为多种屏幕尺寸设计

虽然我们通过使用适当的屏幕单位使 GameView 能够在所有屏幕尺寸和宽高比下均等缩放，但 Android 视图的工作方式并非如此；不过，我们可以为它们遵循标准的 Android 程序。

对于应用，建议根据屏幕尺寸调整布局。对于游戏，在所有屏幕尺寸下保持相同布局，仅更改某些项目的大小和边距更有意义。

### 注意

游戏应为所有屏幕尺寸保持相同布局。

作为经验法则，你应始终使用 RelativeLayout 来设计布局，其中屏幕上的每个项目都相对于其他某个项目定位。这允许布局平滑适应所有屏幕尺寸。这是响应式设计的核心概念。

为了在所有屏幕尺寸下获得正确的显示，我们将限定尺寸；这包含在 Android SDK 中，是推荐做法。

### 注意

限定尺寸是应用和游戏的最佳实践。

限定资源是通过根据屏幕尺寸选择不同的尺寸规范并动态应用它们来工作的。请注意，自 Android 3.2 以来，使用 normal、large 和 xlarge 关键字限定资源已被视为过时（尽管这是查看文档时首先发现的内容）。

有三种基于屏幕尺寸限定资源的方法：

- 最小宽度：无论设备是横向还是纵向，都取设备的最小宽度。当你需要保证任何尺寸的最小大小时很有用。屏幕旋转时不会改变。
- 可用屏幕宽度：当前屏幕宽度。请注意，方向改变时这会发生变化，其他资源（如布局）也可能随之改变。
- 可用屏幕高度：与前一种互补。

使用这些的方法是创建一个资源目录，并附加限定词，例如 values-sw720dp 或 values-w820dp。请注意，这些限定词以 dips 定义。

- 普通手机的值介于 320x480dp 和 480x800dp 之间，因为尺寸可从 3.2 英寸变化到超过 5 英寸。
- 7 英寸平板电脑大约从 600x960dp 开始。
- 10 英寸平板电脑通常至少有 720x1280dp。

我们将根据可用屏幕宽度限定一些尺寸，并将手机、小型平板电脑和大型平板电脑分开。我们将使用目录 values-w820dp（向导已创建）和 values-w1024dp。

让我们看看目前为游戏定义的尺寸：

```
<resources>
    <dimen name="square_button_padding">18dp</dimen>
    <dimen name="square_button_border">6dp</dimen>
    <dimen name="round_button_padding">6dp</dimen>
    <dimen name="btn_round_size">60dp</dimen>

    <dimen name="menu_margin">8dp</dimen>
    <dimen name="game_frame_width">6dp</dimen>
    <dimen name="game_menu_width">100dp</dimen>

    <dimen name="life_size">30dp</dimen>

    <dimen name="activity_horizontal_margin">16dp</dimen>
    <dimen name="activity_vertical_margin">16dp</dimen>

    <dimen name="dialog_width">400dp</dimen>
    <dimen name="dialog_height">250dp</dimen>
</resources>
```

请注意，由于我们的 TextViews 使用标准 Android 样式，我们没有文本大小的尺寸。如果你为文本使用自己的样式，限定文本大小也是可取的。

我们不会修改触摸目标的大小。因此，我们将为所有尺寸保持正方形和圆形按钮尺寸。

### 注意

触摸目标的大小不应随屏幕尺寸而改变。

边距是 Android Studio 在创建默认项目时为 w820dp 定义的，因此我们也不需要修改这些。

游戏的边距和对话框的大小是我们需要更新的尺寸。仅此一项就会让 7 英寸和 10 英寸平板电脑上的效果好得多。

手机的最坏情况是最窄的手机，当其宽度为 480 dp 时，因为这只留下 280 dp 的游戏区域。即便如此，这也超过了屏幕的 50%，看起来还可以。

另一方面，当游戏宽高比过宽时，布局开始看起来很差。因此，对于 w820dp，我们将改用 150 dp 作为边距，而对于 w1280dp，我们将使用 200 dp。你可以轻松在 Android Studio 的预览工具中调整这些值，直到你喜欢结果为止。生命大小也应该更大，因为在这些屏幕上三个生命的空间也更大。

我们也会在更大的屏幕上稍微增大对话框。

总的来说，values-w820dp 将有以下 dimens.xml：

```
<dimen name="activity_horizontal_margin">64dp</dimen>
<dimen name="menu_margin">12dp</dimen>

<dimen name="game_menu_width">150dp</dimen>
<dimen name="life_size">45dp</dimen>

<dimen name="dialog_width">500dp</dimen>
<dimen name="dialog_height">300dp</dimen>
```

而对于 values-w1080dp，我们将有这些：

```
<dimen name="menu_margin">16dp</dimen>
```
