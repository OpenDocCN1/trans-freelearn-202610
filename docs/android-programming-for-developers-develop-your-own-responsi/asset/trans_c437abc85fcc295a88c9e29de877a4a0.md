

## 提供主屏幕横幅

Android TV 应用必须在清单中声明横幅。这是移动应用不存在的另一项要求。

横幅被 Leanback UI 用于在应用和游戏之间导航。它与图标的宽高比不同。具体来说，横幅是一张 320x180 px 的图片，我们需要将其放在 drawable-xhdpi 目录下。

![](img/35d01831c19688a44e50bce7c489007a_1482_0.png)

*横幅是一张 320x180 px 的图片*

这张图片对 TV 来说与图标对移动设备同样重要，它是你在 TV 上的游戏入口。虽然我们作为开发者可以在不提供此横幅的情况下在 TV 上安装和运行应用，但如果不提供横幅，就无法从 Leanback UI 轻松启动应用。

要定义使用哪张图片作为横幅，我们需要将其作为 AndroidManifest.xml 中 `<application>` 标签的属性之一添加。

```xml
<application
[...]
android:banner="@drawable/banner_small" >
```

此属性是在 Lollipop 中引入的，这是我们需要以 target SDK 21 进行编译才能发布到 Android TV 的原因之一。

![](img/35d01831c19688a44e50bce7c489007a_1483_0.png)

模拟器中 Leanback UI 里的我们的游戏

## 声明其为游戏

如前一张截图所示，Leanback UI 将应用与游戏分开，而我们的游戏被列在应用中。我们正在构建一个游戏，希望被归类为游戏。

这是 `<application>` 标签的另一个属性，同样是在 Lollipop 中为 Android TV 引入的；我们可以将应用声明为游戏。

```
<application
[...]
android:isGame="true">
```

这将把横幅显示在 **游戏** 类别中。

![](img/35d01831c19688a44e50bce7c489007a_1484_0.png)

YASS 在 ADT-1 的游戏类别中列出

## 声明 Leanback 支持

为了让 Google Play 在从 Android TV 搜索时列出我们的游戏，我们需要专门声明我们支持 Leanback UI。为此，我们需要声明我们使用 Leanback 功能：

```
<uses-feature android:name="android.software.leanback"
    android:required="false" />
```

需要注意的是，我们声明使用它，但将其标记为非必需。这很重要，因为否则游戏将不会出现在没有 Leanback 功能的设备上。如果我们有两个单独的 APK，应该仅在 TV 构建变体上将此功能标记为必需。

声明 uses-feature 意味着你的应用使用它。将其标记为非必需意味着它可以在没有它的情况下运行，但如果存在，它将会使用它。

### 注意

声明 uses-feature 意味着你的应用使用它。将其标记为非必需意味着它可以在没有它的情况下运行。

如果你为移动设备和 TV 使用单独的 APK，应该仅在 TV 构建变体上声明 Leanback 的 uses-feature，而对移动设备不做任何操作。这样做，TV APK 将仅在具有 Leanback UI 的设备上出现，而移动 APK 将仅在不支持 Leanback 的设备上出现。

Leanback 功能的默认状态是非必需。

## 声明触摸屏功能为非必需

由于 uses-feature 系统的工作方式，我们需要修正请求的功能，以便让我们的游戏在 Android TV 上可用。基本上这意味着我们不能请求 TV 没有的功能，否则游戏不会显示为可用。

这看起来不像那么简单，因为有些功能在不做任何操作的情况下会被自动包含。

由于 Android 的特性，触摸屏功能默认是必需的，但如果我们不将其标记为非必需，游戏将不会在 Android TV 的 Google Play 上列出，因为该设备没有触摸屏。

### 注意

触摸屏功能在 Android 应用中默认是必需的。

这很容易修复，我们只需在 AndroidManifest.xml 上声明触摸屏功能为非必需。

```
<uses-feature android:name="android.hardware.touchscreen"
android:required="false" />
```

需要注意的是，这也意味着游戏在没有触摸屏的情况下应该完全可用。为此，我们在本章后面有一个关于使用控制器处理菜单的专门章节。

Android TV 上还有其他不可用的功能，如果你使用它们，必须确保将其标记为非必需（并在它们不可用时处理相应情况）。这些功能包括：

- 触摸屏：android.hardware.touchscreen
- 触摸屏模拟器：android.hardware.faketouch
- 电话：android.hardware.telephony
- 相机：android.hardware.camera
- 蓝牙：android.hardware.bluetooth
- 近场通信 (NFC)：android.hardware.nfc
- GPS：android.hardware.location.gps
- 麦克风：android.hardware.microphone
- 传感器：android.hardware.sensor

仅就游戏而言，触摸屏和传感器方面可能令人担忧，但有些游戏将位置和/或相机作为游戏不可或缺的一部分。在将游戏带到 TV 上时，这可能是一个决定性障碍。

### 注意

如果你的游戏依赖 GPS 或相机，它可能不太适合在 TV 上运行。

此外，某些 uses-permission 声明隐含了对硬件功能的要求。隐含要求功能的权限包括：

- RECORD_AUDIO 需要使用 android.hardware.microphone。
- CAMERA 需要：
  - android.hardware.camera
  - android.hardware.camera.autofocus
- ACCESS_COARSE_LOCATION 需要：
  - android.hardware.location
  - android.hardware.location.network
- ACCESS_FINE_LOCATION 需要：
  - android.hardware.location
  - android.hardware.location.gps

如果你使用任何这些权限，你应该确保将相应的功能标记为非必需，并在不存在时相应处理。

要在运行时检测某个功能是否存在以启用或禁用游戏的一部分，你可以使用 PackageManager 的 hasSystemFeature 方法。例如：

```
getPackageManager().hasSystemFeature("android.hardware.touchscreen"
)
```

需要注意的是，getPackageManager 在 Context 级别可用，因此你可以从 Activity 中访问它。

## 检查清单

一旦我们将 TV 的活动添加到 Leanback 启动器，确保 Leanback 和触摸屏功能被请求但非必需，配置了横幅，并将其标记为游戏，我们就完成了对 AndroidManifest.xml 的修改。

清单的更新版本如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest
xmlns:android="http://schemas.android.com/apk/res/android"
package="com.plattysoft.yass" >
<application
android:icon="@mipmap/ic_launcher"
android:label="@string/app_name"
android:isGame="true"
android:banner="@drawable/banner_small"
android:theme="@style/AppTheme" >
<uses-feature android:name="android.software.leanback"
android:required="false" >
<uses-feature android:name="android.hardware.touchscreen"
android:required="false" >
<meta-data android:name="com.google.android.gms.games.APP_ID"
android:value="@string/app_id" >
<meta-data android:name="com.google.android.gms.version"
android:value="@integer/google_play_services_version">
<activity
android:screenOrientation="sensorLandscape"
android:name=".YassActivity"
android:hardwareAccelerated="false"
android:label="@string/title_activity_main" >
<intent-filter>
<action android:name="android.intent.action.MAIN" >
<category android:name="android.intent.category.LAUNCHER" >
</intent-filter>
</activity>
<activity
android:name=".YassTvActivity"
android:label="@string/app_name" >
<intent-filter>
    <action android:name="android.intent.action.MAIN" >
    <category
    android:name="android.intent.category.LEANBACK_LAUNCHER" >
    </intent-filter>
</activity>
</application>
</manifest>
```

## 显示控制器说明

要让应用获得 Android TV 批准的特殊要求之一是提供一个指示控制器映射方式的对话框。为此，Google 为我们提供了一个可用的模板，可在线查看：(http://developer.android.com/training/tv/games/index.html)。

![](img/35d01831c19688a44e50bce7c489007a_1490_0.png)

控制器映射模板

我们已经有一个在连接控制器时显示的对话框，但我们从未将其从普通 AlertDialog 更新过。是时候将其改为自定义对话框了。

手机的逻辑是在主菜单首次加载且控制器连接时仅显示一次对话框。对于 Android TV，我们希望有不同的行为：每次打开游戏时都显示对话框。

为此，我们将覆盖 MainMenuFragment 上的 showControllerHelp 方法，以使用带有修改后的模板图片的自定义对话框。

