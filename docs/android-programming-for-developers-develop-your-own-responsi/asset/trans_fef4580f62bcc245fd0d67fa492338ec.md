

Bitmap photo = (Bitmap) data.getExtras().get("data");
imageView.setImageBitmap(photo);
}
}

我们现在准备好测试我们的小应用了。

## 运行 Simple Photo 应用

如果你在模拟器上运行此应用，那么现在是一个稍微更改模拟器配置以提高兼容性的好时机。

为此，请点击 Android Studio 菜单栏中的 AVD Manager 图标。点击铅笔图标以编辑模拟器。点击 **高级设置** 按钮。找到 **相机** 部分。并将 **前置** 和 **后置** 选项都设置为 **模拟**。

运行应用并点击 **拍照** 按钮。你会看到设备的默认相机应用已打开。你可以使用与正常拍照时相同的所有选项来拍摄照片。但是，拍摄照片后，系统会提供保存或丢弃图像的选项：

![](img/35d01831c19688a44e50bce7c489007a_726_0.png)

如果你选择丢弃它（通过点击叉号图标），那么在调用 onActivityResult 时，resultCode == RESULT_OK 将为 false，if 语句主体中的代码将不会被执行。

但是，如果你选择保存它（通过点击对勾图标），那么你拍摄的照片将如预期般显示在 ImageView 中：

![](img/35d01831c19688a44e50bce7c489007a_727_0.png)

前两个图看起来有点奇怪。它们展示了模拟器如何处理相机使用。如果你在真实设备上运行此应用，你将能够拍摄真实照片。

让我们将这种功能添加到 Where it's snap 中，但也让我们走得更远一些。

## Where it's snap – 编码捕获 Fragment

现在我们可以将捕获功能添加到 Where it's snap 应用中。我们将实现从相机拍摄图像的功能，并允许用户为图像添加标题和标签。但是，我们不会添加保存任何这些数据的功能，因为在这样做之前，你首先需要了解数据库。

但是，为保存数据做准备，我们将遇到 Android Uri 和 File 类，它们允许我们识别和定位用户捕获的照片。

本章结束时的完整应用可以在 chapter 22/Where Its Snap 2 中找到。

我们将首先添加 CaptureFragment 将使用的布局，然后添加一些代码（其中一些代码到现在应该看起来很熟悉），以拍摄图像。接下来的两章将研究数据库以保存和显示我们所有的数据。

## 为捕获 Fragment 创建布局

如果你愿意，可以手动添加所有字符串资源和 id 属性，但最快的方法是将下一段代码复制到 capture_fragment.xml 文件中。但是，请务必审查代码，并牢记 id 属性，因为当我们稍后编码 CaptureFragment 类时，这将使你更容易跟进。

将此代码添加到 capture_fragment.xml 文件中：

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="match_parent"
android:layout_height="match_parent">

<Button
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="@string/capture"
android:id="@+id/btnCapture"
android:layout_marginBottom="28dp"
android:layout_alignParentBottom="true"
android:layout_alignParentLeft="true"
android:layout_alignParentStart="true"
android:layout_marginLeft="25dp"
android:layout_marginStart="25dp" >

<ImageView
android:layout_width="150dp"
android:layout_height="150dp"
android:id="@+id/imageView"
android:src="@mipmap/ic_launcher"
android:layout_below="@+id/editTextTitle"
android:layout_centerHorizontal="true" >

<Button
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="@string/save"
android:id="@+id/btnSave"
android:layout_alignTop="@+id/btnCapture"
android:layout_alignParentRight="true"
android:layout_alignParentEnd="true"
android:layout_marginRight="22dp"
android:layout_marginEnd="22dp" >

<EditText
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/editTextTitle"
    android:hint="@string/enter_photo_title"
    android:layout_alignParentTop="true"
    android:layout_centerHorizontal="true" >

<EditText
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/editTextTag1"
    android:hint="@string/tag1"
    android:layout_marginBottom="121dp"
    android:layout_above="@+id/btnCapture"
    android:layout_alignLeft="@+id/btnCapture"
    android:layout_alignStart="@+id/btnCapture" >

<EditText
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/editTextTag2"
    android:hint="@string/tag2"
    android:layout_alignTop="@+id/editTextTag1"
    android:layout_alignLeft="@+id/editTextTag1"
    android:layout_alignStart="@+id/editTextTag1"
    android:layout_marginTop="59dp" >

<EditText
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/editTextTag3"
    android:hint="@string/tag3"
    android:layout_above="@+id/btnCapture"
    android:layout_alignLeft="@+id/btnCapture"
    android:layout_alignStart="@+id/btnCapture" >

</RelativeLayout>
```

请查看并确保你的布局如下图所示。但是，不必完全一致，id 属性必须一致，并且为了使第 27 章“本地化 – Hola！”中的代码正确工作，我们需要使用字符串资源（如前述代码所示），而不是对任何内容进行硬编码：

![](img/35d01831c19688a44e50bce7c489007a_731_0.png)

## 编码捕获 Fragment

由于我们所做的准备工作，这段代码的大部分都相当简单，但我会在新代码出现时逐一指出。

首先，我们需要向 AndroidManifest.xml 文件添加一些更多权限。现在打开它，并在下一个代码清单中添加高亮显示的行：

```
<?xml version="1.0" encoding="utf-8"?>
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.gamecodeschool.whereitssnap2" >

    <uses-permission
        android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

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
</manifest>
```

正如我们很快就会看到的，我们将创建自己的文件，用于保存照片。这就是为什么我们需要 WRITE_EXTERNAL_STORAGE 权限的原因。

现在，向 CaptureFragment 类添加四个高亮显示的成员变量：

```
public class CaptureFragment extends Fragment {

    private static final int CAMERA_REQUEST = 123;
    private ImageView mImageView;

    // The filepath for the photo
    String mCurrentPhotoPath;

    // Where the captured image is stored
    private Uri mImageUri = Uri.EMPTY;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }
```

在接下来显示的 onCreateView 方法中，我们将获取每个 UI 小部件的引用，包括 ImageView、两个 Button 小部件和四个 EditText。

然后我们添加一个匿名类来监听用户点击 **拍摄** 按钮。在 onClick 方法中，我们使用与小应用中 onClick 方法相同的代码。此次唯一的区别是，操作发生在 onCreateView 中而不是 onCreate 中，处理 Fragment 时需要如此。

此外，我们通过调用我们很快将编写的 createImageFile 方法为我们的图像创建一个新文件。之后，我们使用 Uri.fromFile 方法获取一个指向新创建文件的 URI。我们将在第 24 章“向 Where It's Snap 添加数据库”中详细讨论 URI 是什么，届时我们将使用它们在数据库中存储图像。目前，我们可以将它们视为指向图像的链接。

请注意，然后我们使用 putExtra 方法将相同的 Uri 添加到 Intent 类。这将告诉相机应用将此 uri 指向图像保存的位置。实际上，我们现在有了一个指向将要捕获的图像的链接。

请注意，接下来的代码不处理 **保存** 按钮。在你几章后学习如何处理数据时，我们将处理它。

将我们刚刚讨论的代码添加到 onCreateView：

```
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

