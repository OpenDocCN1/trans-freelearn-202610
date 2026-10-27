

# 处理 Web 应用程序

执行以下步骤以启用拖放

+   1. 我们想要拖放的元素必须在 HTML 中获得 `draggable` 属性，因此在 `drag_drop.html` 中，我们对棋盘的 `<div>` 元素进行了如下指示：
+   
+   ```html
+   <div id="tiles">
+       <div id="tile1" class="pict" draggable="true"></div>
+           <div id="tile2" class="pict" draggable="true"></div>
+           ...
+       </div>
+   ```

+   2. 在配套脚本 `drag_drop.dart` 中，我们创建一个类 `Drag nDrop`，以整合所有拖放事件处理。我们的所有 `<div>` 瓷砖都有 CSS 类 `pict`，因此被捕获在 `cols` 集合中。`main()` 函数创建一个对象 `dnd`，并在其上调用 `init()` 方法：
+   
+   ```dart
+   void main() {
+       var dnd = new Drag nDrop();
+       dnd.init();
+   }
+   
+   class Drag nDrop {
+       Element dragSource;
+       Map tiles;
+       var rand = new Random();
+       var cols = document.querySelectorAll('.pict');
+       // ...
+   }
+   ```

+   3. `init()` 函数在 `generateMap()` 中构造一个包含 img 文件夹中图像的映射。它为我们的瓷砖设置所有 Dnd 事件的监听器，并且通过调用 `getRandomTile()`，图像通过 `style.setProperty` 设置为瓷砖的背景图像：
+   
+   ```dart
+   void init() {
+       generateMap();
+       for (var col in cols) {
+           col
+               ..onDragStart.listen(_onDragStart)
+               ..onDragEnd.listen(_onDragEnd)
+               ..onDragEnter.listen(_onDragEnter)
+               ..onDragOver.listen(_onDragOver)
+               ..onDragLeave.listen(_onDragLeave)
+               ..onDrop.listen(_onDrop)
+               ..style.setProperty("background-image", getRandomTile(),
+                   "")
+   ```

```
..style.setProperty("background-repeat", "no-repeat", "")
..style.setProperty("background-position", "center", "");
}
}

void generateMap() {
    tiles = new Map();

    for (var i = 1; i <= 9; i++) {
        tiles[i] = "url(img/tiles_0$i.jpg)";
    }
}

String getRandomTile() {
    var num = rand.nextInt(10);
    var imgUrl = tiles[num];
    while (imgUrl == null) {
        num = rand.nextInt(10);
        imgUrl = tiles[num];
    }
    tiles.remove(num);
    return imgUrl;
}
```

4. 每个 Dnd 事件处理程序的代码如下：

```
void _onDragStart(MouseEvent event) {
    Element dragTarget = event.target;
    dragTarget.classes.add('moving');
    dragSource = dragTarget;
    event.dataTransfer.effectAllowed = 'move';
    // event.dataTransfer.setData('text/html', dragTarget.innerHtml);
}

void _onDragEnd(MouseEvent event) {
    Element dragTarget = event.target;
    dragTarget.classes.remove('moving');
    for (var col in cols) {
        col.classes.remove('over');
    }
}

void _onDragEnter(MouseEvent event) {
```

+   - 数据负载是我们试图放置的内容
+   - 目标元素是用于捕获放置或放置区域的区域，它接受用户试图放置的数据

几乎所有东西都可以启用拖动，包括图像、文件、链接或其他 DOM 节点。然而，并非所有元素都可以作为目标，例如图像，但我们的示例表明，很容易绕过这一点。

伴随拖放的视觉效果是通过 CSS 实现的。每个瓷砖都有 CSS 类 `pict`，其中包含属性 `cursor: move`，这为用户提供了视觉提示，表明某些东西可以移动。

当拖动操作开始时，会执行 `_onDragStart`，为我们的瓷砖添加 CSS 类 `moving`。该类可以在 `drag_drop.css` 中找到：

```
.pict.moving {
  opacity: 0.25;
  -webkit-transform: scale(0.8);
  -moz-transform: scale(0.8);
  -ms-transform: scale(0.8);
  transform: scale(0.8);
}
```

我们看到不透明度降低到 0.25，大小按 0.8 的因子缩放，这正是我们在拖动操作开始时看到的效果。在本示例中我们不需要它，但如果拖动涉及 HTML 文本，`_onDragStart` 应包含以下代码行：

```
event.dataTransfer.setData('text/html', dragTarget.innerHtml);
```

前面的代码指示在拖放过程中将传输哪些数据。`dataTransfer` 属性是所有操作发生的地方；它存储在拖动操作中发送的数据片段。`dataTransfer` 选项在 `DragStart` 事件中设置，并在 `Drop` 事件中读取/处理。调用 `e.dataTransfer.setData(format, data)` 将设置对象的 MIME 类型内容，并将数据负载作为参数传递。

当我们悬停在另一块瓷砖上时，会执行 `_onDragEnter` 函数；这会添加 CSS 类 `over`，使边框线变为虚线：

```
.column.over {
  border: 2px dashed #000;
}
```

最后，`_onDrop` 事件交换瓷砖图像。如果需要交换 HTML 文本，应添加以下行：

```
dragSource.innerHtml = dropTarget.innerHtml;
dropTarget.innerHtml = event.dataTransfer.getData('text/html');
```

Eric Bidelman 在 http://www.html5rocks.com/en/tutorials/dnd/basics/ 的文章是一个很好的资源，但它使用 JavaScript 来解释 DnD 事件模型。为了更轻松地在 Dart 中使用 DnD，Marco Jakob 开发了可在 pub 上获取的库 dart-html5-dnd。有关这个不错的包的更多信息，请参阅 http://code.makery.ch/dart/html5-drag-and-drop/。DnD 功能也在 dart:svg 库中实现。

## 另请参阅

+   - 请参阅本章中 *启用触摸事件* 的配方，了解如何使用触摸实现拖放

## 启用触摸事件

在没有连接鼠标的移动设备上，拖放非常方便；我们只有手指与屏幕交互。本配方将向您展示如何通过触摸事件为您的 Web 应用添加交互性。实现方法与上一配方非常相似。我们将重用拖放示例。

### 如何操作...

让我们按照以下步骤查看触摸项目：

+   1. 使用 touch.html 中的以下 <meta> 标签防止缩放：

```
<meta name="viewport"
content="width=device-width, initial-scale=1.0, user-scalable=no">
```

我们使用相同的 <div> 结构，每个都具有 class = "draggable"，如上一配方所示。

+   2. 在 touch.dart 中，我们创建一个类 Touch，用于包含特定于触摸事件的代码。创建一个对象 touch 并调用 init() 方法，该方法准备棋盘（每个瓷砖获得不同的背景图像）并将触摸事件绑定到事件处理程序：

```
void main() {
  var touch = new Touch();
  touch.init();
}

class Touch {
```

```
Element dragSource;
Map tiles;
var rand = new Random();
var cols = document.querySelectorAll('.pict');

void init() {
    generateMap();
    for (var col in cols) {

col
    ..onTouchStart.listen(_onTouchStart)
    ..onTouchEnd.listen(_onTouchEnd)
    ..onTouchMove.listen(_onTouchMove)
    ..style.setProperty("background-image", getRandomTile(), "")
    ..style.setProperty("background-repeat", "no-repeat", "")
    ..style.setProperty("background-position", "center", "");  }
    }
```

3. 以下是 TouchStart 事件的代码：

```
void _onTouchStart(TouchEvent event) {
    event.preventDefault(); //stop scrolling by default
    Element dragTarget = event.target; //capture drag target
    //add style to element to indicate its moving
    dragTarget.classes.add('moving');
    dragSource = dragTarget;
}
```

4. 以下是 TouchMove 的代码：

```
void _onTouchMove(TouchEvent event) {
    event.preventDefault();
    Element dropTarget = event.target;
    dragSource.classes.add('moving');
    // Get the current x,y position of the first finger touch  //
    and find the element it is over
    dropTarget = document.elementFromPoint(event.touches[0].page.x, event.touches[0].page.y);
    // If the finger is over an element indicate that in the UI
    if (dropTarget != null) {
        dropTarget.classes.add('over');
    }
}
```

