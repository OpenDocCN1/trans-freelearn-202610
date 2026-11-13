

## BaseCustomDialog

我们将要创建的自定义对话框框架由三个项目组成：

- BaseCustomDialog: 所有对话框的基类
- my_overlay_dialog: 所有对话框的父布局
- 基础片段类中的实用函数

大部分工作都在 BaseCustomDialog 类中完成，该类提供 setContentView、show 和 dismiss 等方法。

BaseCustomDialog 类的完整代码如下：

```java
public class BaseCustomDialog implements OnTouchListener {

    private boolean mIsShowing;

    protected final YassActivity mParent;
    private ViewGroup mRootLayout;
    private View mRootView;

    public BaseCustomDialog(YassActivity activity) {
        mParent = activity;
    }

    protected void onViewClicked() {
        // Ignore clicks on this view
    }

    protected void setContentView(int dialogResId) {
        ViewGroup activityRoot = (ViewGroup)
                mParent.findViewById(android.R.id.content);
        mRootView = LayoutInflater.from(mParent).inflate(dialogResId,
                activityRoot, false);
        mParent.applyTypeface(mRootView);
    }

    public void show() {
        if (mIsShowing) {
            return;
        }
        mIsShowing = true;

        ViewGroup activityRoot = (ViewGroup)
                mParent.findViewById(android.R.id.content);
        mRootLayout = (ViewGroup)
                LayoutInflater.from(mParent).inflate(R.layout.my_overlay_dialog,
                        activityRoot, false);
        activityRoot.addView(mRootLayout);
        mRootLayout.setOnTouchListener(this);
        mRootLayout.addView(mRootView);
    }

    public void dismiss() {
        if (!mIsShowing) {
            return;
        }
        mIsShowing = false;
        hideViews();
    }

    private void hideViews() {
        mRootLayout.removeView(mRootView);
        ViewGroup activityRoot = (ViewGroup)
                mParent.findViewById(android.R.id.content);
        activityRoot.removeView(mRootLayout);
    }

    protected View findViewById(int id) {
        return mRootView.findViewById(id);
    }

    @Override
    public boolean onTouch(View v, MotionEvent event) {
        // Ignoring touch events on the gray outside
        return true;
    }

    public boolean isShowing() {
        return mIsShowing;
    }
}
```

在构造过程中，我们将主活动的引用存储到成员变量 mParent 中，以便稍后使用。

该类中还有几个其他成员变量：

- mRootLayout: 这是我们为所有对话框通用并用作背景的布局。
- mRootView: 内容视图的根视图。这用于显示和隐藏对话框本身。
- mIsShowing: 用于确定对话框是否显示的变量。

在 setContentView 期间，我们会为对话框填充视图并将引用存储在 mRootView 中，但暂不将其添加到任何 ViewGroup。在填充时，我们使用应用内容视图的参数 (android.R.id.content)。inflate 方法的最后一个参数用于确定系统是否应将视图作为父项的子视图添加。我们传递 false，因此视图不会被添加到层次结构中。最后，我们会将自定义字体应用到该视图。

需要注意的是，setContentView 通常在构造期间调用，而不是在 show 期间调用；这就是为什么此时视图未被添加到层次结构中的原因。

要显示对话框，我们首先检查它是否已经显示；如果是，则返回且不执行任何操作。否则，我们将 mIsShowing 设置为 true 并继续。

为了正确显示对话框，我们填充根布局并将其添加到主内容中，然后将根视图添加到它。这会使对话框成为视图层次结构前景中的最前端项目。

我们还将类添加为 mRootLayout 上不执行任何操作的触摸监听器。其目的是过滤掉对话框后方项目中的所有触摸和点击事件。如果我们希望当用户点击对话框边界之外时关闭对话框，只需在此监听器中添加对 dismiss 的调用即可。

最后，dismiss 首先检查对话框是否显示，如果是，则从层次结构中移除视图。

我们还拥有 findViewById 和 isShowing 等实用方法。

mRootLayout 的布局只是一个灰色半透明遮罩层，其代码如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:background="#aa000000"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

需要注意的是，这已经为显示和关闭对话框的过程添加动画做好了设计（将在下一章讨论）。这在拥有与内容视图独立的完全灰色背景时尤其适用。

在我们的对话框框架中，最后一个阶段是在基础片段中提供一些实用方法，以方便使用自定义对话框。

```java
BaseCustomDialog mCurrentDialog;

public void showDialog(BaseCustomDialog newDialog) {
    showDialog(newDialog, false);
}

public void showDialog(BaseCustomDialog newDialog, boolean dismissOtherDialog) {
    if (mCurrentDialog != null && mCurrentDialog.isShowing()) {
        if (dismissOtherDialog) {
            mCurrentDialog.dismiss();
        } else {
            return;
        }
    }
    mCurrentDialog = newDialog;
    mCurrentDialog.show();
}

public boolean onBackPressed() {
    if (mCurrentDialog != null && mCurrentDialog.isShowing()) {
        mCurrentDialog.dismiss();
        return true;
    }
    return false;
}
```

我们将通过在片段上调用 showDialog 方法来显示对话框。这将存储当前显示的对话框的引用，可用于确定是否有其他内容正在显示，并在需要时将其关闭。

默认情况下，如果屏幕上已有另一个对话框显示，我们将不再显示新对话框，但如果你想在游戏中这样做，还有一个版本的 showDialog 接收布尔参数，用于在存在其他对话框时将其关闭。

我们还处理了返回键按压事件，并在显示对话框时使用它们关闭对话框，并返回 true 以表示该事件已被片段消耗。

现在我们已经具备了所有基础，让我们为游戏制作一个退出对话框。

## Quit dialog

退出对话框必须在 MainMenuFragment 中按下返回键时出现。它将显示一些文本和两个按钮，用于退出或继续游戏。

这是一个简单的对话框，在开始创建更复杂的对话框之前，它将帮助我们理解所有组件如何融入架构中。

我们将在 layouts 文件夹下创建 dialog_game_over.xml 文件来定义它。我们使用一种命名约定，为所有对话框布局定义赋予以 dialog 开头的名称（类似于片段和活动的命名约定）。

布局如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_gravity="center"
    android:background="@drawable/dialog_bg"
    android:layout_width="@dimen/dialog_width"
    android:layout_height="@dimen/dialog_height">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textColor="@color/text_color"
        android:layout_marginTop="@dimen/activity_vertical_margin"
        android:layout_centerHorizontal="true"
        style="@android:style/TextAppearance.Large"
        android:text="@string/exit_confirm" />

    <LinearLayout
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="@dimen/activity_vertical_margin"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">
        <ImageView
            style="@style/iconButton"
            android:layout_marginRight="@dimen/btn_sound_size"
            android:id="@+id/btn_resume"
            android:src="@drawable/resume" />
        <ImageView
            style="@style/iconButton"
            android:id="@+id/btn_exit"
            android:src="@drawable/exit" />
    </LinearLayout>
</RelativeLayout>
```

这相当简单，但我们仍有几点需要说明：

- 按钮的 ID 将为 btn_exit 和 btn_resume，它们使用从免费图标集获取的透明图像。
- 两个按钮都使用我们为主屏幕创建的 iconButton 样式，该样式设置了圆形状态列表背景图像。
- 为了使按钮居中，我们将它们放置在 LinearLayout 中，而该 LinearLayout 本身居中于 RelativeLayout。这是居中多个按钮的最简单方法。
- 我们使用 drawable dialog_bg 作为背景。它是一个 shape drawable，类似于方形按钮的背景，但颜色不同。
- 对话框的宽度和高度已作为尺寸提取。它们对所有对话框都相同。我们已将其设置为 400x250dp。

继承自 BaseCustomDialog 的类也相当简单：

```java
public class QuitDialog extends BaseCustomDialog implements View.OnClickListener {
    private QuitDialogListener mListener;

    public QuitDialog(YassActivity activity) {
        super(activity);
        setContentView(R.layout.dialog_quit);
        findViewById(R.id.btn_exit).setOnClickListener(this);
        findViewById(R.id.btn_resume).setOnClickListener(this);
    }

    public void setListener(QuitDialogListener listener) {
        mListener = listener;
    }

    @Override
    public void onClick(View v) {
        if (v.getId() == R.id.btn_exit) {
            dismiss();
            mListener.exit();
        }
    }
}
```

