
## Where it's snap 应用

Where it's snap 将是我们最先进的应用！它将具备以下功能：

- 它将按标题显示照片列表。
- 它将提供标签列表作为替代方案，点击标签后将跳转到包含该匹配标签的标题列表。
- 它将允许用户使用设备相机拍照，并分配标题以及相关标签。
- 所有所需数据（标题和标签）将存储在数据库中。你将在 [第 23 章，在我们的应用中使用 SQLite 数据库] 中了解数据库，我们将在第 [第 24 章，向 Where It's Snap 添加数据库] 中为 Where it's snap 添加数据库功能。
- 当用户在列表中点击照片标题时，他/她将看到该照片，并且还会看到显示拍摄地点 Google 地图的选项。
- 我们还将了解如何使此应用支持多语言（西班牙语和英语）。Hola！

与 Note To Self 应用一样，我们会尽可能正确地做事，并在代码中使用字符串资源和 Android 命名约定。

我们将在未来六个章节中构建这个应用。在探索和练习 Android API 新功能的过程中，我们将通过改进或向 Where it's snap 添加功能来利用它们。

我们要做的第一件事是准备项目的资源并实现导航抽屉。这将是本项目 UI 的基本部分。

代表本章结束时此应用可运行版本的项目文件，可以在下载包中的 chapter 21/where its snap 1 文件夹中找到。

像往常一样，创建一个名为 where_its_snap 的新项目并接受所有默认设置。

为了处理 Marshmallow 运行时权限，如 [第 11 章，Android 权限与棉花糖中的微件狂热] 中讨论的，我们需要将目标 API 设置为 22。

要做到这一点，从项目资源管理器顶部的下拉列表中选择 **Android**。现在，双击项目资源管理器窗口底部附近的 **build.gradle (module: app)** 选项。

更改突出显示的代码行，以确保 targetSdkVersion 设置为 22，如以下代码片段所示：

```
defaultConfig {
    applicationId "com.gamecodeschool.whereitssnap"
    minSdkVersion 15
    targetSdkVersion 22
    versionCode 1
    versionName "1.0"
}
```

现在，我们可以添加未来六个章节所需的资源了。

## 准备资源

在本节中，我们将准备字符串资源。为此，从 res 文件夹中打开 strings.xml 文件。在整个项目期间，我们不需要再次返回此文件。有些资源看起来可能有点奇怪，确实可能不会立即使用，但如果你回头查看项目的功能，将很容易猜测这些字符串资源最终将在哪里使用。不过，我们将在第 27 章，本地化 – Hola！中创建另一个 strings.xml 文件，届时我们将使应用支持多语言。

除了常规字符串资源外，还有一个 name 属性设置为 nav_drawer_items 的 string-array 元素。我们很快就会看到，在我们的 Java 代码中如何像使用常规 Java 数组一样使用它，就像我们在第 13 章，处理和显示数据数组中讨论的那些一样。

修改 strings.xml 文件的内容，使其与以下代码相同：

```
<resources>
<string name="app_name">Where It's Snap</string>
<string name="action_settings">Settings</string>
<string name="drawer_open">Open navigation drawer</string>
<string name="drawer_close">Close navigation drawer</string>
<string name="enter_photo_title">Enter photo title</string>
<string name="tag1">Tag 1</string>
<string name="tag2">Tag 2</string>
<string name="tag3">Tag 3</string>
<string name="capture">Capture</string>
<string name="save">Save</string>
<string name="tags">Tags</string>
<string name="titles">Titles</string>
<string name="show_map">Show Map</string>
<!-- These are the items in our Navigation drawer -->
<string-array name="nav_drawer_items">
<item>Titles</item>
<item>Tags</item>
<item>Capture</item>
</string-array>
</resources>
```

我们现在几乎准备好实现导航抽屉了。在此之前，让我们添加一些可用于抽屉各个选项的空占位符 Fragment。

## 三个 Fragment 的占位符代码

如我们从计划中所知，共有四个 Fragment：图像捕获、标题列表、标签列表和显示图片。

前三个可以直接从主导航抽屉 UI 访问。对于这三个 Fragment，我们将快速创建占位符类和布局，以便让 MainActivity 正常运作。然后，在接下来的章节中，我们将添加真正的功能和布局。

这三个 Fragment 中的两个（标题视图和标签视图）将是可点击选项的列表，因此使用 ListFragment 似乎是个不错的选择。第三个（图像捕获视图）更像是一个常规布局，因此 Fragment 是最佳选择。

让我们继续快速创建这些类。

创建一个新类并将其命名为 TitlesFragment。修改代码，使其与以下代码完全相同，其中我们扩展 ListFragment 并实现一个基本的 onCreate 方法，以便在项目中日后使用：

```
public class TitlesFragment extends ListFragment {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

}
```

现在，对于标签列表视图，创建一个新类并将其命名为 TagsFragment。修改代码，使其与以下代码完全相同，其中我们扩展 ListFragment 并实现一个基本的 onCreate 方法，以便在项目中日后使用：

```
public class TagsFragment extends ListFragment {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }
}
```

现在，对于将最终允许用户拍照并添加标题和标签的 Fragment。

创建一个新类并将其命名为 captureFragment。修改代码，使其与以下代码完全相同，其中我们扩展 Fragment 并实现 onCreate 和 onCreateView 的基本版本，以便在项目中日后使用。

以下是 CaptureFragment.java 的代码：

```
public class CaptureFragment extends Fragment{

@Override
public void onCreate(Bundle savedInstanceState){
super.onCreate(savedInstanceState);

}

@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
//Inflate the layout file then get all necessary references
View view = inflater.inflate(R.layout.fragment_capture, container, false);

return view;
}

}
```

### 提示

CaptureFragment 之前的两个类不需要 onCreateView 方法的原因是 ListFragment 内置了 ListView，其功能和外观将在项目后期由数组适配器处理。

现在，我们还需要为 fragment_capture.xml 创建一个占位符布局，以消除先前代码中引用 R.id.fragment_capture 导致的错误。

在 layout 文件夹上右键单击并导航到 **新建 | 布局资源文件**。将其命名为 *fragment_capture* 并单击 **确定**。

最后，将一个单独的 `TextView` 拖放到布局上，并将其 `text` 属性设置为 `Capture`。

我们现在可以在 `MainActivity.java` 中编写代码来让这些 Fragment 活起来。但是，它们目前只是空白，除了我们刚刚添加到 *fragment_capture.xml* 的单个 `TextView`。

## 编码 MainActivity 类和布局

让我们添加一个非常简单但全新的布局，它将定义我们应用的外观。不出所料，该布局将包含我们的导航抽屉。

稍后将出现的布局代码看起来又长又复杂，但仔细检查后，它只是常规 RelativeLayout 中的 DrawerLayout。然后，在 DrawerLayout 内部是一个 id 为 fragmentHolder 的 RelativeLayout。这个 RelativeLayout 就是我们将用于替换各种 Fragment 的地方。然后，仍在 DrawerLayout 内部，我们有 ListView。正是这个 ListView 将保存导航抽屉的所有选项。

请注意，ListView 的 id 属性是 navList。我们将很快了解如何使用这个 id 属性让它活起来（滑入和滑出）。

另外，请注意，我们将 ListView 的宽度属性设置为 200dp。请随意尝试，按喜好将其设得更宽或更窄。

由于前面的两个 UI 元素（RelativeLayout 和 ListView）都没有给出任何相对布局指令，它们都将从屏幕左上角开始并填充整个屏幕，例外情况是 ListView 的宽度仅为 200dp。另外，由于 ListView 在将保存我们 Fragment 的 RelativeLayout 之后声明，它将位于顶层。这正是我们需要的。

以下是我们刚刚讨论过的 activity_main.xml 的代码。编辑 activity_main.xml，使其完全如下所示：

```
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

<android.support.v4.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
```
