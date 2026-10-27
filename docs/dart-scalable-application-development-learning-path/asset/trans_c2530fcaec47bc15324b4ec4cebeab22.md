

希望这能帮助你提高地理知识并了解更多关于 Dart 的信息！

在 Dartium 中调试时，使用 REST 或其他类似的 Web 服务，可以在浏览器开发者工具的控制台窗口中获得 XMLHttpRequests 的额外日志。

调出上下文菜单（右键单击），其中有一个名为 Log XMLHttpRequests 的选项，可在控制台输出窗口中显示网页发出的 HTTP 请求：

[-121.5371704, 36.8165016, 0.93]
129.91739911111114 177.278328
2.2222222222222223 3.3333333333333335
maxWidth 2
XHR finished loading: GET "http://127.0.0.1:8080/api/quake/v1/latest".

在调试项目时，这非常宝贵，可以节省在每个发出 HTTP 请求的代码部分添加日志的工作。

## 放大显示

canvas 元素具有一系列 2D 绘图功能，其中包括一个缩放功能，我们可以利用它来聚焦地图上最活跃的部分，即北美。在 main.dart 中，有一个名为 zoomed 的标志，默认为 false，可帮助应用程序跟踪当前显示状态。一个按钮绑定到缩放代码实现：

```dart
zoomMap(Event evt) async {
  if (zoomed == true) {
    zoomed = false;
    quakeMap.mapCtx.resetTransform();
    quakeMap.mapCtx.scale(1, 1);
  } else {
    zoomed = true;
    quakeMap.mapCtx.scale(2, 2);
  }
  quakeMap.drawMapGrid();
}
```

## 实时可视化

canvas 在缩放设置之间切换，其中对 `resetTransform` 方法的调用至关重要。如果没有它，缩放将无法恢复正常。整个地图视图必须立即重绘，以便用户获得响应式体验。让我们看一下以下屏幕截图：

![](img/52c05b53a56abd53584e4fed738ed509_210_0.png)

如果你观看此页面一段时间，你会发现加利福尼亚和阿拉斯加的特征在数据中占据很大比重。随着弹出代码处理缩放变化，这些特征仍然可用。第二次按下 **Zoom** 按钮会返回到完整的世界视图。

> print 函数不仅仅用于命令行应用程序，也可用于 Web 应用程序。输出显示在 **Developer Tools** 控制台日志窗口中，可通过右键单击页面调出上下文菜单，然后单击 `Inspect Element` 来显示。

## 实时可视化

一旦获得许可，最后一步是创建通知类的实例。不需要进一步的方法调用：

![](img/52c05b53a56abd53584e4fed738ed509_212_0.png)

应用程序设置为经常通知——你可能希望提高通知地震的级别，否则你会发现自己需要 dismiss 很多通知。通知的视觉显示在不同平台和 Web 浏览器上可能有所不同。

## 绘制用户位置

大多数移动设备都配备了地理位置设备，或者可以通过 IP 等次要信息甚至回退到用户输入来确定位置。这通过 HTML5 地理位置 API 在 `http://www.w3.org/TR/geolocation-API/` 上向现代 Web 开发者公开以供应用程序使用。

与桌面通知一样，用户必须授予页面使用其位置的权限。另一个小障碍是，位置功能在 Dartium 上不起作用。要尝试此应用程序的功能，请使用 pub build 创建 JavaScript 版本并使用其他浏览器：

![](img/52c05b53a56abd53584e4fed738ed509_212_1.png)

页面上的 **Locate** 按钮将尝试获取用户的位置并通过将其添加到 `featurePlotter` 类实例在页面上绘制它：

```dart
locateUser(Event evt) async {
  Geoposition geoPos = await window.navigator.geolocation.
  getCurrentPosition();
  MapIndicator mapIndicator;
  var pos =
  featPlotter.toPoint(geoPos.coords.longitude, geoPos.coords.
  latitude);
  mapIndicator = new MapIndicator(pos.x, pos.y, quakeMap.mapCtx, 4);
  mapIndicator.colorPrimary = "#0000ff";
  mapIndicator.colorSecondary = "#00ffff";
  featPlotter.userLocation.add(mapIndicator);
}
```

坐标的转换方式与地震相同，并且指示器更改为蓝色，以便在地图上突出显示，如以下屏幕截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_213_0.png)

一旦应用程序编译为 JavaScript，在获得用户许可的情况下，地理位置数据即可使用，蓝色点指示用户的位置，在本例中是相对地震较少的英国。

在命令行和 Dartium 中运行的 Dart VM 并不是唯一的体现。Dart 团队正在开发两个在其他环境中运行的 Dart 实验版本，与主 Dart VM 有很大不同。目前，两者都运行在移动环境中，并专注于并发和高性能。

Fletch 在桌面操作系统上运行，但旨在在 Android 和 iOS 上执行且没有 JIT。Fletch 对高度异步的 Dart 采取了 somewhat 相反的方法，偏爱用户级线程，这些线程被阻塞但只保留少量资源。Fletch 为在不同平台上共享同一代码提供了有趣的前景；这将对移动开发者来说是巨大的生产力提升。

https://github.com/dart-lang/fletch

Sky 仅在撰写时为 Android，并雄心勃勃地旨在以 120 FPS 运行高度响应式的应用程序。为了实现这一点，它优先考虑用户界面，以便在进行其他处理时仍能保持 8 ms 的响应。目前，它可能可以在 Dart VM 可以运行的任何地方运行。

https://github.com/domokit/sky_engine/tree/master/sky/packages/sky

如果 Fletch 和 Sky 原型成功，它们很可能将是移动或服务器虚拟机，而不是嵌入在 Web 服务器中。

## 对功能列表进行排序

到目前为止，坐标列表中的第三个数据，即地震深度，还没有被使用。当用户按下 Sort 按钮时，此数据将以当前功能的排序列表形式呈现。让我们看一下以下代码片段：

```dart
void sortFeatures(Event evt) {
  featPlotter.sortFeatures();
  DivElement out = querySelector('#depthDetail');
  out.nodes.clear();
  featPlotter.geoFeatures.forEach((feature) {
    LIElement detail = new LIElement();
    detail.innerHtml = "${feature[5]}km - ${feature[3]}";
    out.nodes.add(detail);
  });
}
```

## 使用 dartdoc 记录 Dart 代码

文档是软件开发的关键部分，为了鼓励对包进行良好的文档记录，Dart 提供了工具 dartdoc (https://github.com/dart-lang/dartdoc/)，它基于特殊格式的代码注释创建静态 HTML 文档。Dart 之前的文档工具是 SDK 的一部分，而 dartdoc 是 Dart 团队开发的独立项目。

它可以通过命令行使用 pub 安装：

```bash
pub global activate dartdoc
```

我们将查看一个 Dart 包项目以探索 dartdoc 的功能。在 WebStorm 编辑器中打开项目 quakerecord，并查看文件 quakerecord.dart：

```dart
library quakerecord;
export 'src/quakerecord_base.dart';
```

该包导出单个文件 quakerecord_base.dart，在此文件中声明了类 Processor：

```dart
/// A lightweight object to process a raw earthquake feature.
///
/// * Must have valid JSON.
/// * Must be less than 128MB.
///
class Processor {

// Store for the JSON.
String _feature;

/// The state of processing
bool get processed => true;

/// The default constructor.
///     Processor processor;
///     processor = new Processor('{}');
Processor(String feature) {}

/// Returns a converted object based on the feature passed into
/// the constructor [Processor].
object convert() {
    // A normal comment.
```

