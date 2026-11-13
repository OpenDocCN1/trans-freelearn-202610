
## 在 MainActivity 中编写片段通信代码

我们只需实现 ActivityComs 接口并添加 DataManager 类的一个实例。然后，我们处理接口的两个方法，就可以开始了。

按如下高亮显示的代码实现接口并添加 DataManager 的实例：

```
public class MainActivity extends ActionBarActivity implements ActivityComs{
    private ListView mNavDrawerList;
    private DrawerLayout mDrawerLayout;
    private ArrayAdapter<String> mAdapter;
    private ActionBarDrawerToggle mDrawerToggle;
    private String mActivityTitle;

    public DataManager dataManager;
```

在 onCreate 中添加高亮显示的代码行来初始化 DataManager 实例：

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    dataManager = new DataManager(getApplicationContext());

    // We will come back here in a minute!
    mNavDrawerList = (ListView)findViewById(R.id.navList);
    mDrawerLayout = (DrawerLayout)findViewById(R.id.drawerLayout);
    mActivityTitle = getTitle().toString();

    ...
```

现在，我们来编写 onTagsListItemSelected 方法。在这里，我们将传入的表示标签的字符串放入 Bundle，并传递给 TitlesFragment 的新实例。我们已经了解了 TitlesFragment 如何处理这个字符串。

按讨论内容实现接口所需的 onTagsListItemSelected 方法：

```
public void onTagsListItemSelected(String clickedTag){
    // We have just received a String for the TitlesFragment

    // Prepare a new Bundle
    Bundle args = new Bundle();

    // Pack the string into the Bundle
    args.putString("Tag", clickedTag);

    Create a new instance of TitlesFragment
    TitlesFragment fragment = new TitlesFragment();

    // Load the Bundle into the Fragment
    fragment.setArguments(args);

    // Start the fragment
    FragmentManager fragmentManager = getFragmentManager();
    fragmentManager.beginTransaction().replace(R.id.fragmentHolder,
    fragment, "TAGS").commit();

    // update selected item and title, then close the drawer
    mNavDrawerList.setItemChecked(1, true);
    mNavDrawerList.setSelection(1);
    mDrawerLayout.closeDrawer(mNavDrawerList);
}
```

在 onTitlesListItemSelected 方法中，我们接收了一个 int，它表示 wis_table_photos 表中一行的 _id，需要在 ViewFragment 中向用户显示。

代码将 int 添加到 Bundle，然后在启动 Fragment 之前将 Bundle 添加到 Fragment。

实现接口的 onTitlesListItemSelected 方法：

```
// Open ViewFragment with the photo indicated by position
public void onTitlesListItemSelected(int position) {
    // Load up the bundle with the row _id
    Bundle args = new Bundle();
    args.putInt("Position", position);

// Create the fragment and add the bundle
ViewFragment fragment = new ViewFragment();
fragment.setArguments(args);

// Start the fragment
if (fragment != null) {
    FragmentManager fragmentManager = getFragmentManager();
    fragmentManager.beginTransaction().replace(R.id.fragmentHolder,
    fragment, "VIEW").commit();

    // update selected item and title, then close the drawer
    mNavDrawerList.setItemChecked(1, true);
    mNavDrawerList.setSelection(1);
    //setTitle(navMenuTitles[position]);
    mDrawerLayout.closeDrawer(mNavDrawerList);
} else {
    // error in creating fragment
    Log.e("MainActivity", "Error in creating fragment");
}
}
```

这是一个代码量很大的章节，但现在我们有幸看到我们的应用程序运行起来了。

## 到目前为止运行应用程序

向数据库中添加一些照片、标签和标题，然后运行你的应用程序。你

![](img/35d01831c19688a44e50bce7c489007a_802_0.png)

将看到以下内容：

在 **标签** 屏幕上，点击一个标签以查看包含该标签的照片标题列表：

- 2015
- 巴哈马
- 乐趣
- 秋天
- 步行
- 英国
- 风景
- 风景
- 2016
- 假期
- 可爱

标签：

点击标题以在 ViewFragment 中查看照片：

- 吉姆在度假
- 玛里奥特大道
- 诺福克湖
- 诺福克湖
- 诺福克湖
- 神秘小径
- 巴哈马海滩
- 克里斯在冲浪板上

## 总结

这可能是迄今为止代码量最大的章节。有可能这个工作原理的每个细节都不完全清楚。让事情更清晰的方法是逐步构建项目。回顾第 19 章 *使用多个片段* 以复习接口，也可能会有所帮助。

如果你的知识中有几个模糊的地方，这不必阻止你继续前进，因为本书的其余部分仍将提供大量不需要掌握本章内容的 Android API 精华。

接下来，我们将探讨如何将 GPS 位置和 Google Maps 集成到我们的应用程序中。

## 第 25 章. 集成 Google Maps 和 GPS 位置

在本章中，我们将走向全球，甚至简要涉足太空。我们将看到 Android API 如何与 GNSS（全球导航卫星系统）协同工作以提供位置数据。然后，我们可以使用这些数据在 Google Maps 上绘制位置。

在本章中，我们将：

- 了解 GPS 的基础知识
- 构建一个 GPS 小型应用程序
- 了解 Google Maps
- 连接到 Google Maps 服务器以获取地图数据

让我们先快速了解一下 GPS。

## 全球定位系统

GPS 是那些让你在坐下来思考其工作原理时总能让你感到惊叹的技术之一。当你还考虑到你可以放在口袋里的手机也能使用它时，这就更是令人难以置信的非凡。

### 提示

警告：如果你出生于 1990 年之后，可能无法理解上一段，并且可能认为 GPS 相当乏味。

该系统与太空中称为 GNSS 的 27 颗卫星协同工作。其中，24 颗卫星处于活动状态，三颗是备份。每颗卫星每 12 小时绕地球运行一圈，不断广播变化的位置数据。

通过对至少三颗这些卫星的数据进行计算，我们的设备可以为我们提供世界上的经度和纬度位置。稍微简化一下（好吧，是大幅简化），这些是相对于极点和赤道的度数。它们是非常精确的值，正如我们将看到的，因此可能精确到五米。

正如我们已知的第 1 章《第一个应用程序》，所有复杂性都由 Android API 隐藏了起来，现在我们可以构建一个超级简单的应用程序来查看 GPS 的运行情况。

## 世界在哪里 – GPS 小型应用程序

创建一个新项目并将其命名为 where in the world。

如第 11 章“控件狂想曲”中关于 Android 权限和 Marshmallow 的部分所讨论，为了处理 Marshmallow 运行时权限，我们需要将目标 API 设置为 22。

为此，从项目资源管理器顶部的下拉列表中选择 **Android**。现在，双击项目资源管理器窗口底部附近的 **build.gradle (module: app)** 选项。

确保也更改高亮显示的代码行，以便 targetSdkVersion 设置为 22，如下面的代码片段所示：

```
defaultConfig {
    applicationId "com.gamecodeschool.whereitssnap3"
    minSdkVersion 15
    **targetSdkVersion 22**
    versionCode 1
    versionName "1.0"
}
```

现在，我们可以将所需权限添加到 AndroidManifest.xml 文件中。

打开文件，如下面的代码中高亮所示添加三个权限：

```
<?xml version="1.0" encoding="utf-8"?>
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.gamecodeschool.whereintheworld" >
    <uses-permission
    android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission
    android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.INTERNET" />
    <application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
