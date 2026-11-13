
## 使用相机捕获图像

Android 提供了两种编写使用相机的应用的方法：

- 一种方法是从头开始编写相机应用，并利用设备可能拥有的任何相机的任何或全部功能。这是一个大话题，通常仅在应用设计为添加某种新相机功能时才使用。
- 另一种方法是使用现有的相机应用为我们完成工作。

我们将通过使用设备的现有相机应用（选项 2）来“作弊”（以一种好的方式）。我们可以使用我们在第 15 章 Android Intent 和 持久性 中首次看到的 Intent 类来完成此操作。

通过仔细构建传递给 Intent 类构造函数的参数，我们可以让其他应用为我们完成工作。我们将看到捕获图像的代码，但同样的简单技术也可用于发送电子邮件、在社交媒体平台上发布消息、播放媒体等，远不止于此。

要让 Intent 类启动相机应用，我们将使用 android.provider.MediaStore.ACTION_IMAGE_CAPTURE 构建一个 Intent 类。

我们所需要做的就是像这样构建一个 Intent 类：

```
Intent intent = new Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
```

完成此操作后，我们使用新的 Intent 对象启动一个 Activity，但与第 15 章 Android Intent 和 持久性 中调用 startActivity 不同，这次我们调用 startActivityForResult，以便新 Activity 知道我们希望将结果（一些数据）返回给我们的 Activity。

我们将像这样启动 Intent 类：

```
startActivityForResult(cameraIntent, CAMERA_REQUEST);
```

然后，我们可以在 onActivityResult 方法中捕获结果（希望是一张图片），这是 Android 在这些情况下会调用的方法。我们可以借助以下代码来实现：

```
Bitmap photo = (Bitmap) data.getExtras().get("data");
```

在前面的代码中，我们创建了一个新的 Bitmap（图像）对象，并将返回的数据赋值给它。前面的解释中有一些注意事项和缺失部分，最好通过演示而非解释来展示。让我们看看如何在快速小应用中使所有这些正常工作。

当我们着手编写 Where it's snap 应用的图像捕获功能时，我们会将事情推得更远。

## 捕获图像小应用

创建一个新项目并将其命名为 Simple Photo。和往常一样，可以将所有设置保留为默认值。

如第 11 章 Widget Mania 中 Android 权限和 Marshmallows 部分所讨论，为了处理 Marshmallow 运行时权限，我们需要将目标 API 设置为 22。

为此，在项目资源管理器顶部的下拉列表中选择 Android。现在，双击项目资源管理器窗口底部的 build.gradle (module: app) 选项。

更改高亮代码行，使 targetSdkVersion 设置为 22，如下面的代码所示：

```
defaultConfig {
    applicationId "com.gamecodeschool.simplephoto"
    minSdkVersion 15
    targetSdkVersion 22
    versionCode 1
    versionName "1.0"
}
```

现在，我们需要请求用户允许使用相机。

## 将相机权限添加到清单中

我们需要做的第一件事是编辑 AndroidManifest.xml 文件。这样做是因为我们需要请求用户允许访问他们的相机。当我们向 AndroidManifest.xml 文件添加权限时，会触发通知用户我们的应用打算使用其相机，并在安装时请求其许可。此外，我们可以将相机的使用添加为一个功能，这样我们的应用就会对没有相机的设备用户隐藏。

幸运的是，添加相机权限和功能很容易。从项目资源管理器的 manifests 文件夹中打开 AndroidManifest.xml 文件。添加下面代码中显示的高亮行：

```
<?xml version="1.0" encoding="utf-8"?>
<manifest
xmlns:android="http://schemas.android.com/apk/res/android"
package="com.gamecodeschool.simplephoto" >
<application
android:allowBackup="true"
android:icon="@mipmap/ic_launcher"
android:label="@string/app_name"
android:theme="@style/AppTheme" >
<activity
android:name=".MainActivity"
android:label="@string/app_name" >
<intent-filter>
<action android:name="android.intent.action.MAIN" >
<category android:name="android.intent.category.LAUNCHER" >
</intent-filter>
</activity>
</application>
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera" />
</manifest>
```

现在，我们可以处理布局。

## 定义简单布局

这个布局非常基础。当我们在本章后面将图片捕获功能添加到 Where it's snap 时，我们会花更多时间和精力来布局捕获 Fragment：

```
<LinearLayout
xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">

<Button android:id="@+id/button1"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="Take picture">

<ImageView android:id="@+id/imageView1"
android:layout_height="wrap_content"
android:layout_width="wrap_content">

</LinearLayout>
```

前面的布局包含 id 属性为 button1 的 Button 和用于放置新捕获图像的 ImageView，其 id 属性为 imageView1。

现在，我们可以开始在 MainActivity 中编写 Java 代码了。

## 编写 MainActivity 类

首先，我们需要添加两个成员变量：

- 我们有一个名为 CAMERA_REQUEST 的 `final int`。当我们启动一个新 Activity 并期望返回一些数据时，必须为请求发送一个 ID。使用 `final` 变量是一种良好的做法，因为当我们在 `onActivityResult` 中接收数据时，需要确定自己处理的是相机数据。每次调用 `startActivityForResult` 时，响应都会返回到 `onActivityResult`。我们将看到如何验证我们实际处理的是认为的那种数据。在这个简单的应用中，我们理所当然地认为会从相机应用接收数据，但随着我们的应用变得更加复杂，情况并非总是如此。
- ImageView 将用于显示返回的 Bitmap。

添加两个高亮的成员变量：

```
public class MainActivity extends AppCompatActivity {
    private static final int CAMERA_REQUEST = 123;
    private ImageView imageView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

接下来，在 onCreate 方法中，我们获取布局中 ImageView 和 Button 的引用。

然后，我们编写一个匿名类来监听 Button 的点击。请注意，onClick 方法会创建前面讨论过的新 Intent 类，并调用 startActivityForResult，传入 Intent 类以及我们的 id 属性 CAMERA_REQUEST。

现在，将刚才讨论的代码添加到 onCreate 方法中。它在下一个代码清单中高亮显示：

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    imageView = (ImageView)this.findViewById(R.id.imageView1);
    Button photoButton = (Button) this.findViewById(R.id.button1);
    photoButton.setOnClickListener(new View.OnClickListener() {

        @Override
        public void onClick(View v) {
            Intent cameraIntent = new
            Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
            startActivityForResult(cameraIntent, CAMERA_REQUEST);
        }});
    }
```

最后，我们可以添加至关重要的 onActivityResult 方法，它捕获作为参数传入的 Intent 类中的结果（一张照片）。请注意这里的 int requestCode 参数，它应与我们传入 startActivityForResult 的 id 属性匹配。还要注意 resultCode，它也是 int。

我们使用 if 语句来测试两点：

- 这是我们认为的那种数据吗（requestCode == CAMERA_REQUEST）？
- Intent 类中是否实际上包含任何适当的数据（resultCode == RESULT_OK）？

Activity 可能失败，也有可能用户改变了主意，没有拍照。if 语句会处理这些可能性。

如果一切正常，执行将进入 if 语句体，我们从 Intent 类中声明并初始化一个新 Bitmap，然后使用 setImageBitmap 方法将其显示在 ImageView 上。

添加刚才讨论的 onActivityResult 方法：

```
protected void onActivityResult(int requestCode, int resultCode,
Intent data) {

if (requestCode == CAMERA_REQUEST && resultCode == RESULT_OK) {

Bitmap photo = (Bitmap) data.getExtras().get("data");
```
