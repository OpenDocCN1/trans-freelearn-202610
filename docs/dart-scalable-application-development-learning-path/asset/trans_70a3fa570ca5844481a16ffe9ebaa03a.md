

API 使用纬度和经度坐标来表示位置。

> 地理定位 API 不保证返回设备的真实位置。

地理定位 API 包含以下类：

- `Geolocation`：此类用于确定与宿主设备关联的位置信息
- `Geoposition`：此类用于存储坐标和时间戳
- `Coordinates`：此类用于存储设备的位置信息和速度

## 确定当前位置

让我们看看如何使用地理定位 API 获取当前位置的信息：

```
import 'dart:html';

void main() {
  window.navigator.geolocation.getCurrentPosition()
    .then((Geoposition geoposition) {
      querySelector("#latitude").text = geoposition.coords.latitude.toStringAsFixed(6);
      querySelector("#longitude").text = geoposition.coords.longitude.toStringAsFixed(6);
    }, onError: (PositionError error) {
      print(error.message);
    });
}
```

我们可以从 `window` 对象的 `navigator` 属性中获取 `geolocation` 实例。`geolocation` 的 `getCurrentPosition` 方法在 `Future` 对象中返回 `Geoposition`。当 Future 被解析时，我们将把 `geoposition` 的坐标赋值给纬度和经度的 HTML 字段。

> Dartium 不支持地理定位，因此请运行 pub serve 并使用 Chrome 浏览器进行测试。

- 3. 选择**推荐**选项，并通过点击**管理例外**按钮打开**地理位置例外**对话框。目前，该对话框中不会包含我们的网站，如以下截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_501_0.png)

- 4. 运行地理定位应用。Chrome 浏览器会询问你是否允许使用你的位置信息，如以下截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_501_1.png)

5. 你可以允许或拒绝该网站的位置请求。你的选择将决定后续权限请求如何处理 `geoposition` 的值。网站随后会被添加到**地理位置例外**列表中。请再次打开**地理位置例外**对话框以查看你的网站：

![](img/52c05b53a56abd53584e4fed738ed509_502_0.png)

`geolocation` 服务中发生的任何错误都会被打印出来：

```
Network location provider at 'https://www.googleapis.com/'
: Returned error code 400.
```

6. 现在，我们将从项目的根目录运行 `pub serve`：

```
Loading source assets...
Serving geolocation web on http://localhost:8080
Build completed successfully
[web] GET /geolocation.html => geolocation|web/geolocation.html
[web] GET /geolocation.css => geolocation|web/geolocation.css
[web] GET /packages/browser/dart.js => browser|lib/dart.js
[Info from Dart2JS]:
Compiling geolocation|web/geolocation.dart...
[Info from Dart2JS]:
Took 0:00:27.723586 to compile geolocation|web/geolocation.dart.
Build completed successfully
[web] GET /geolocation.dart.js => geolocation|web/geolocation.dart.js
[web] GET /geolocation.dart.js.map => geolocation|web/geolocation.dart.js.map
```

- 7. 接着，在 Chrome 浏览器中打开该 Web 应用，会得到以下结果：

![](img/52c05b53a56abd53584e4fed738ed509_503_0.png)

如果加入 Google Maps 在真实地图上显示你的当前位置，这个示例会更有趣。

## 地图上的地理定位

将 google_maps 包添加到你的项目中，并对代码进行以下修改：

```
import 'dart:html';
import 'package:google_maps/google_maps.dart';

void main() {
  final mapOptions = new MapOptions()
    ..zoom = 15
    ..mapTypeId = MapTypeId.ROADMAP;
  final map = new GMap(querySelector("#map_canvas"), mapOptions);

  window.navigator.geolocation.getCurrentPosition()
    .then((Geoposition geoposition) {
      Coordinates coords = geoposition.coords;
      querySelector("#latitude").text = coords.latitude.toStringAsPrecision(6);
      querySelector("#longitude").text = coords.longitude.toStringAsPrecision(6);
      //
      map.center = new LatLng(coords.latitude, coords.longitude);
    }, onError: (PositionError error){
      print(error.message);
    });
}
```

## 支持其他 HTML5 特性

在 mapOptions 中，我们指定了缩放级别和视图类型。地图可以以卫星、地形或混合视图呈现。使用 Google Maps API 非常简单。只需在 HTML 页面中添加带有指定 ID 的 div 元素，然后执行 pub serve 命令来编译代码并运行服务器。当你在 Chrome 浏览器中打开**地理位置**页面时，将得到以下结果：

![](img/52c05b53a56abd53584e4fed738ed509_504_0.png)

## 跟踪当前位置

地理定位 API 可以使用 watchPosition 方法监控设备的当前位置。通过 enableHighAccuracy 参数，地理定位 API 会开始使用设备上可用的更精确的硬件。该方法返回一个 geoposition 坐标流。你只需监听事件即可跟踪当前位置的变化。代码如下：

```
import 'dart:html';
import 'package:google_maps/google_maps.dart';
```

```
void main() {
    final mapOptions = new MapOptions()
    ..zoom = 25
    ..mapTypeId = MapTypeId.ROADMAP;
    final map = new GMap(querySelector("#map_canvas"), mapOptions);

    window.navigator.geolocation
    .watchPosition(enableHighAccuracy:true)
    .listen((Geoposition geoposition) {
        Coordinates coords = geoposition.coords;
        map.center = new LatLng(coords.latitude, coords.longitude);
        //
        querySelector("#location_tracker").append(new DivElement()
        ..text = coords.latitude.toStringAsPrecision(6) + " x " +
        coords.longitude.toStringAsPrecision(6));
    }, onError: (PositionError error){
        print(error.message);
    });
}
```

当你运行该 Web 应用时，会得到以下结果：

![](img/52c05b53a56abd53584e4fed738ed509_505_0.png)

## Canvas

HTML5 canvas 是一个非常出色的功能，它允许你通过代码进行编程式绘图操作。它之所以变得非常流行，是因为它允许你在网页中直接创建和处理图像。canvas 是 HTML5 新特性中最灵活的标签之一。这个标签就像一块空白的画板。它定义了一个上下文对象，用户可以在其中绘制图形。实际的绘制操作可以分为以下几种方式：

- 绘制 2D 上下文
- 绘制 3D 上下文（正式名称为 WebGL）

所有现代网络浏览器都支持 2D 上下文。它更为成熟和稳定，而 3D 上下文仍处于早期的定义阶段。由于 2D 上下文得到更广泛的支持，我们来讨论一下它。

canvas API 既简单又强大。你只能通过脚本在 2D 位图表面上绘制。在绘制过程中，你所绘制的形状不会对应任何 DOM 节点。你生成的只是像素，因此即使图形复杂度增加，也无需担心性能下降。

在 canvas 上绘图，本质上就是在屏幕上的适当坐标处添加像素。通常，屏幕上像素的坐标与 canvas 中用网格表示的点一一对应，但当我们放大或缩小屏幕，或通过 CSS 调整 canvas 的大小时，这些坐标可能会发生变化。网格的关键点是位于 canvas 左上角的原点，其坐标为 (0, 0)。绘制在 canvas 上的每个形状都有一个 x 轴和 y 轴的偏移量，以及宽度和高度属性。

## 示例 —— canvas 编辑器

canvas API 允许你执行以下操作：

- 绘制矩形、椭圆、线条等形状
- 渲染文本
- 像素操作
- 在区域、形状或文本中填充颜色
- 创建渐变和图案以填充区域、形状或文本
- 复制图像、其他 canvas 或视频帧
- 将 canvas 内容导出到文件

