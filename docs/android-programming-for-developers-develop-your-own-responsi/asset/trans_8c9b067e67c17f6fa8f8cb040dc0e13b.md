

```java
// The SurfaceView class implements onTouchListener
// So we can override this method and detect screen touches.
@Override
public boolean onTouchEvent(MotionEvent motionEvent) {
    switch (motionEvent.getAction() & MotionEvent.ACTION_MASK) {
        // Player has touched the screen
        case MotionEvent.ACTION_DOWN:
            mPaused = false;

            // Is the touch on the right or left?
            if(motionEvent.getX() > mScreenX / 2){
                mPaddle.setMovementState(mPaddle.RIGHT);
            }else{
                mPaddle.setMovementState(mPaddle.LEFT);
            }

            break;
        // Player has removed finger from screen
        case MotionEvent.ACTION_UP:
            mPaddle.setMovementState(mPaddle.STOPPED);
            break;
    }
    return true;
}
```

如果你在想我们为什么在 `MotionEvent.ACTION_DOWN` 情况下将 `mPaused` 设置为 `false`，那是因为当玩家的生命耗尽时我们会暂停游戏。当玩家点击屏幕时，这将重新开始游戏。

我们就快完成了。

### 锁定屏幕方向并设为全屏

我们可以通过在 `AndroidManifest.xml` 文件中添加高亮显示的行来实现这一点：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest
xmlns:android="http://schemas.android.com/apk/res/android"
package="com.gamecodeschool.pong" >

<application
android:allowBackup="true"
android:icon="@mipmap/ic_launcher"
android:label="@string/app_name"
android:theme="@style/AppTheme" >
<activity
android:name=".MainActivity"
android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
android:screenOrientation="landscape"

android:label="@string/app_name" >
<intent-filter>
<action android:name="android.intent.action.MAIN" >

<category android:name="android.intent.category.LAUNCHER"
>
</intent-filter>
</activity>
</application>

</manifest>
```

我们就可以开始游戏了！

### 运行游戏

这类应用在真实设备上通常效果更好（如果你有一台的话），但它们在模拟器上也能正常运行：

![](img/35d01831c19688a44e50bce7c489007a_906_0.png)

你可能会发现，当得分大约达到 15 分时（在模拟器上可能更低），游戏会变得相当具有挑战性。

### Pong 增强

虽然我们的游戏相当简约，但我们已经了解足够多的 Android API 知识来构建主屏幕和也许设置屏幕。我们也可以轻松实现一个高分小数据库。

## 摘要

在本章中，我们涵盖了大量内容——线程、绘制、触摸和基本游戏。我已经建议了你可以在哪里获取有关线程和绘制的更多信息，所以希望你能原谅我做一次不厌其烦的自我推销，推荐我另一本专注于 Android 游戏的书籍。如果你想构建射击游戏、平台游戏和像“ flappy bird ”这样的游戏，并学习如何以每秒六十帧的速度绘制超快图形，请查看 https://www.packtpub.com/game-development/android-game-programming-example 上的《Android Game Programming by Example》。

现在我们可以规划和构建多种类型的应用了，我们最好去了解如何在 Google Play 市场上发布和销售它们。

## 第 29 章. 发布应用

这是最短的章节之一，其中完全没有任何技术内容。但是，完成它需要一段时间。在本章中，在上传应用之前，我们将准备好所有所需资源，以及导出可发布的 APK 文件。

本章将涵盖以下主题：

- 准备发布
- 构建可发布的 APK 文件
- 发布应用
- 推广应用的基础知识

### 准备发布

你可能不想上传本书中的任何应用，因此第一步是开发一个你想要发布的应用。

前往 https://play.google.com/apps/publish/ 并按照说明获取 Google Play 开发者账户。撰写时这需要 25 美元，是一次性收费，没有应用发布数量限制。

### 创建应用图标

如何设计图标超出了本书的范围。但简单来说，你需要为每种 Android 屏幕密度分类创建一张漂亮的图片。

这比听起来容易。在你最喜欢的绘图程序中设计一个漂亮的应用图标，并将其保存为 .png 文件。然后，访问 [Android 资源工作室启动器图标](http://romannurik.github.io/AndroidAssetStudio/icons-launcher.html)。这将把你的单个图标转换为适用于每种屏幕密度的完整图标集。

### 提示

警告！使用此服务的代价是，该网站会出于自身营销目的收集你的电子邮件地址。

有很多网站提供类似的免费服务。从前面的网站下载 .zip 文件后，你可以将下载中的 res 文件夹简单地复制到资源管理器中的主项目文件夹。所有密度的所有图标现在都已更新。

### 准备所需资源

当我们登录 Google Play 以在商店中创建新条目时，没有什么技术需要处理，但我们确实需要准备相当多的图片来上传。

为应用兼容的每种设备类型（手机/平板电脑/电视/手表）准备最多 8 个屏幕截图。不要裁剪或填充这些图片。

创建一个 512 x 512 像素的图片，用于在 Google Play 商店中展示你的应用图标。你可以准备自己的图标，或者我们刚才讨论的创建应用图标的过程已经为你自动生成了图标。

你还需要创建三个横幅图形，尺寸如下：

- 1024 x 500
- 180 x 120
- 320 x 180

这些可以是屏幕截图，但通常值得花点时间创建一些更特别的东西。如果你不擅长艺术，可以将屏幕截图放在相当酷的设备图形中，然后添加一张背景图片。你可以在 https://developer.android.com/distribute/tools/promote/device-art.html 生成一些设备图形。

然后，只需在背景中添加你的应用标题或功能即可。下面的横幅完全没有技巧制作而成，只使用了一个花费 10 美元的漂亮背景和我刚才提到的设备图形工具：

![](img/35d01831c19688a44e50bce7c489007a_913_0.png)

此外，考虑为你的应用制作一个视频。除非你的设备已 Root，否则在 Android 设备上录制视频几乎是不可能的。我不建议你 Root 设备；但是，有一个名为 ARC（App Runtime for Chrome）的工具，它允许你在桌面上运行 APK 文件。没有调试输出，但它可以比模拟器更流畅地运行要求较高的应用。然后，使用免费开源的桌面捕获程序（例如 OBS（Open Broadcaster Software））记录你的应用在 ARC 中运行就非常简单了。你可以在 https://developer.chrome.com/apps/getstarted_arc 了解更多关于 ARC 的信息，并在 https://obsproject.com/ 了解 OBS。

### 构建可发布的 APK 文件

在本节中，我们正在准备将上传到 Google Play 的文件。我们将要创建的文件格式是 .apk。这类文件通常被称为 APK。该文件的实际内容是编译后的类文件、我们添加的所有资源，以及 Android Studio 自动生成的文件和资源。我们不需要担心细节，只需按照以下步骤操作即可。

这些步骤不仅创建 APK，还创建一个密钥并使用该密钥为你的应用签名。此过程是必需的，并且还保护你的应用所有权：

### 注意

请注意，这与复制保护/数字版权管理不同。

- 1. 在 Android Studio 中，打开你想要发布的项目，导航到 Build | Generate Signed APK，将打开一个弹出窗口，如下所示：

![](img/35d01831c19688a44e50bce7c489007a_914_0.png)

- 在 Generate Signed APK 窗口中，单击 Create new 按钮。之后，你将看到 New Key Store 窗口，如以下所示。

