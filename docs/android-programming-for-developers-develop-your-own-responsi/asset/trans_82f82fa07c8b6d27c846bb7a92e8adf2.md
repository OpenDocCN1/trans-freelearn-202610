

else if (mGamepadControllerListener != null) {
    if (mGamepadControllerListener.dispatchGenericMotionEvent(event)) {
        return true;
    }
}
return super.dispatchGenericMotionEvent(event);
}

最后，我们需要更新返回键的处理：

@Override
public void onBackPressed() {
if (mCurrentDialog != null && mCurrentDialog.isShowing()) {
mCurrentDialog.dismiss();
return;
}
final YassBaseFragment fragment = (YassBaseFragment)
getFragmentManager().findFragmentByTag(TAG_FRAGMENT);
if (fragment == null || !fragment.onBackPressed()) {
super.onBackPressed();
}
}

如果正在显示对话框，我们直接将其关闭。否则，我们保留之前的代码：让当前片段处理返回键按压，如果事件未被消耗，则将其传递给父类。

请注意，将返回键事件传递给当前片段非常重要。在我们的游戏中，在 GameFragment 中按下返回键意味着我们想要暂停游戏。

为了完成此操作，我们需要让对话框处理按键。BaseCustomDialog 上的默认实现只是返回 false，这意味着事件未被消耗：

public boolean dispatchKeyEvent(KeyEvent event) {
return false;
}
public boolean dispatchGenericMotionEvent(MotionEvent event) {
return false;
}

我们将进行非常简单的处理，只有在对话框上的某个按钮被选中时才处理 OK 点击，但你也可以通过更复杂的处理来防止焦点移动到对话框外部的视图。

对于 ControllerHelpDialog，无论哪个视图获得焦点，只要按下 **确定** 我们就将其关闭：

public boolean dispatchKeyEvent(KeyEvent event) {
    if (event.getKeyCode() == KeyEvent.KEYCODE_BUTTON_A ||
    event.getKeyCode() == KeyEvent.KEYCODE_ENTER ||
    event.getKeyCode() == KeyEvent.KEYCODE_DPAD_CENTER) {
    dismiss();
    return true;
    }
    return false;
}

请注意，从控制器的角度来看，**A** 按钮是 **确定** 按钮，但某些控制器也有 D-Pad 中心按钮，有时还有 *Enter* 键，因此我们将这些按键事件中的任何一个都视为 **确定**。

对于 Quit 对话框，代码略有不同：

public boolean dispatchKeyEvent(KeyEvent event) {
    if (event.getKeyCode() == KeyEvent.KEYCODE_BUTTON_A ||
    event.getKeyCode() == KeyEvent.KEYCODE_ENTER ||
    event.getKeyCode() == KeyEvent.KEYCODE_DPAD_CENTER) {
    if (findViewById(R.id.btn_resume).isFocused() ||
    findViewById(R.id.btn_exit).isFocused()) {
    // Return false, so a proper click is sent
    return false;
    }
    return true;
    }
    return false;
}

在这个对话框中，每当收到 OK 事件时，我们会检查是否有按钮获得焦点。如果是这种情况，我们返回 false，这意味着我们让上层处理它。这将把事件传递给 Activity，在 Activity 中它会被处理并作为点击事件发送回来。

如果我们的操作没有一个获得焦点，我们返回 true，这意味着我们正在消耗该事件，不应该再对它做任何处理。

消耗该事件并对其进行处理应该一起完成。

在任何其他情况下，我们返回 false（同样，不消耗该事件）以让 Activity 正常处理按键事件。

最后，对于在 GameFragment 中显示的暂停和游戏结束对话框，我们不需要做任何事情，因为该 UI 上唯一可聚焦的项目是暂停按钮。

我们将要做的是将暂停按钮设置为不可聚焦，然后让系统处理按键：

<ImageView
 style="@style/iconButton"
 android:focusable="false"
 android:layout_gravity="top|right"
 android:id="@+id/btn_play_pause"
 android:layout_marginTop="@dimen/menu_margin"
 android:layout_marginRight="@dimen/menu_margin"
 android:src="@drawable/pause" />

请注意，虽然 ImageView 默认不可聚焦，但我们已在样式级别设置了该属性，并且可以始终在布局定义中覆盖样式的值。

有了这些更改，我们游戏的所有 UI 都可以使用游戏手柄操作，我们终于准备好在 Android TV 上发布了。

## 超越本书

我们做到了！我们使用 Android SDK 从零开始构建了一个游戏。我们构建了一个具有独立 UpdateThread 和 DrawThread 的游戏引擎，我们有可以移动的精灵，我们处理了触摸和游戏手柄控制器，并添加了碰撞检测、声音，甚至粒子效果。

我们还使用了 Android 框架的组件来构建菜单和对话框，并利用动画工具使其更加动态。

YASS 还使用 Google Play 游戏服务提供了成就和排行榜，现在它也可以在 Android TV 上玩了。

此时，有几件事可以做来改进游戏，如果你愿意，这些可以作为很好的练习来帮助你继续前进。一些想法是：

- 生成难度递增的多波敌人
- 包括更大的小行星，被击中时会分裂成更小的部分
- 添加飞船选择：提供具有不同特性（速度、射速等）的多种飞船
- 实现类似护盾或改进激光的增强道具

虽然本书专注于游戏开发，但制作游戏还有其他被忽视但也非常重要的方面。其中最相关的是游戏设计和变现。

### 注意

游戏设计和变现是制作游戏的关键方面。

**游戏设计** 是一门独立于平台和代码的学科，它需要对游戏本身进行高层次的思考。游戏设计是关于制作一款有趣且能吸引用户的游戏。有许多书籍专门致力于这一概念，且超出了本书的范围。

**变现** 当今也是一个大话题。如果你只为好玩而计划发布游戏，则不需要担心它，但如果你打算从中赚钱，则需要对此进行评估。

变现游戏的主要选项有：

- **付费**：将其设为付费游戏
- **广告**：通过向用户展示广告（横幅或插页广告）来变现游戏
- **应用内购买**：为用户提供在游戏内购买物品的机会

制作付费游戏的优势是构建起来要简单得多，并且会让你少操很多心。

在移动端表现良好的付费游戏示例包括 Monument Valley、Threes 和 The Room（1 和 2）。

免费游戏（无论是带广告还是应用内购买）的优势在于安装门槛最低。如果有人对游戏感兴趣，安装它是免费的。为游戏付费需要做出有意识的决定，许多人会选择退出，无论价格如何。

如果你打算走免费游戏（Free2Play）之路，请准备好担心玩家的留存和转化。你将熟悉 DAU、MAU、ARPU 和 ARPPU 等术语，举几个例子。

采用不同变现方案的成功免费游戏示例包括 Candy Crush Saga、Clash of Clans 和 Angry Birds。

大多数研究会告诉你，人们通过应用内购买赚的钱比付费应用更多。虽然这是真的，但创建一个变现良好的 Free2Play 游戏需要时间和精力来进行正确的调整；设计有吸引力的应用内购买也是如此。

在我的经验中，为 Free2Play 游戏制作良好的变现设计所花费的时间大约与制作游戏本身一样多。如果你不仔细设计，可能会得到非常低的转化率，这会转化为微乎其微或没有任何收入。

作为经验法则，不要在感觉不合适的游戏中强行推行变现模式，而应将精力投入到让游戏变得有趣上。

## 总结

我们的游戏已准备好在 Android TV 上运行。我们更新了清单文件，以便从 Leanback 界面启动一个 Activity 并在 UI 上显示横幅，并将其标记为游戏。我们还审查了权限，以确保游戏可在适用于 Android TV 的 Google Play 上提供，主要是将触摸屏功能和 Leanback 标记为请求但非必需。

从代码角度来看，在 TV 上运行时，我们为所有屏幕添加了一些额外的内边距，以确保重要内容永远不会因为超扫描（overscan）而被截断，并且我们调整了对话框的代码，使其与控制器配合良好。

最后，我们提到了一些关于如何进一步改进游戏的建议，并简要讨论了游戏设计和变现。

是时候开始制作你自己的游戏了。祝你好运，玩得开心！

