

## 拖动反馈图像

通常，原生的拖放 API 会自动创建半透明图像，该图像由 `dragstart` 事件的拖动目标生成，并在拖动操作期间跟随鼠标指针。你可以使用 `dataTransfer` 属性的 `setDragImage` 方法来指定自定义拖动图像。此函数的第一个参数是自定义拖动图像，可以是对真实图像、画布或其他元素的引用。第二个和第三个参数是图像相对于鼠标指针出现时的偏移量。代码如下：

```
var dragSource = querySelector("#sample_drag_id")
  ..draggable = true
  ..onDragStart.listen((MouseEvent event) {
    event.dataTransfer.setData("text/plain", "I'm draggable");
    event.dataTransfer.setDragImage(new
        ImageElement(src:'notification.png'), 0, 0);
  });
```

以下反馈图像将代替标准的半透明图像出现：

![](img/52c05b53a56abd53584e4fed738ed509_494_0.png)

## 拖动效果

拖放 API 支持对可拖动数据执行复制、移动、链接及其组合等操作。我们可以使用复制操作来表示被拖动的数据将从当前位置复制到放置位置。类似地，你可以使用移动操作来表示被拖动的数据将被移动，而链接操作表示将在源位置和放置位置之间创建连接。你应该通过在监听器中设置 `dragstart` 事件的 `effectAllowed` 属性来指定允许并执行的某个操作或组合，如以下代码所示：

```
var dragSource = querySelector("#sample_drag_id")
  ..draggable = true
  ..onDragStart.listen((MouseEvent event) {
    event.dataTransfer.setData("text/plain", "I'm draggable");
    event.dataTransfer.setDragImage(new
        ImageElement(src:'notification.png'), 0, 0);
    event.dataTransfer.effectAllowed = 'copy';
  });
```

在前面的示例中，我们只允许复制操作。让我们看看我们可以将哪些值用作操作的名称：

- none: 此操作表示不允许任何操作
- copy: 此操作表示拖动数据只能从源复制到放置位置
- move: 此操作表示拖动数据只能从源移动到放置位置
- link: 此操作表示拖动数据只能从源链接到放置位置
- copyMove: 此操作表示拖动数据可以从源复制或移动到放置位置
- copyLink: 此操作表示拖动数据可以从源复制或链接到放置位置
- all: 此操作表示拖动数据可以从源复制、移动或链接到放置位置

默认情况下，`effectAllowed` 属性允许全部三种操作。允许的操作可以在 `dragenter` 或 `dragover` 事件的监听器中通过 `effectAllowed` 属性进行检查，并且应该在相关的 `dropEffect` 属性中设置，以指定应执行哪个单一操作。`dropEffect` 属性的有效操作仅为 none、move 或 link，只有这三种，任何其他组合都被禁止。期望的操作会改变鼠标指针，因此对于复制操作，光标可能会出现加号。期望的效果可以通过用户按下修饰键来修改。具体键因平台而异。在 Windows 操作系统中，用户通常使用 Shift 和 Ctrl 键在复制、移动和链接操作之间切换。在 `dragenter` 和 `dragover` 事件期间，我们可以修改 `effectAllowed` 和 `dropEffect` 属性，以指定放置目标支持的操作。`dropEffect` 中指定的效果必须是 `effectAllowed` 属性中列出的其中一个。`dropEffect` 属性的值可以告诉我们拖动操作的确切结果。如果 `dropEffect` 属性的值为 none，则拖动已取消；否则，指定的效果即为已执行的操作。拖放操作在 `dragend` 事件结束之后被视为完成。

> `none` 值可用于任一属性，以表示在目标位置不允许放置操作。

### 放置目标

放置目标是拖动项可能被放置的位置。放置目标非常重要，因为网页的大多数区域不允许放置。`dragenter` 和 `dragover` 事件的事件监听器用于通过取消事件来阻止默认处理，从而指示有效的放置目标，如以下代码所示：

```
var dropTarget = querySelector("#sample_drop_id")
    .onDragOver.listen((MouseEvent event) {
        if (checkTarget(event.target)) {
            event.preventDefault();
            event.dataTransfer.dropEffect = 'copy';
        }
    });
```

在前面的代码中，我们调用 `checkTarget` 方法以确保目标位于可放置的正确位置。在我们的例子中，放置目标必须具有 `droppable` 属性，如以下代码所示：

```
bool checkTarget(Element target) {
    return target.attributes.containsKey('droppable');
}
```

然而，通常放置是否被接受或拒绝取决于 `dataTransfer` 属性中的拖动数据类型。在这种情况下，我们应该检查 `dataTransfer` 属性的类型，以决定是否可以接受数据放置。代码如下：

```
var dropTarget = querySelector("#sample_drop_id")
    .onDragOver.listen((MouseEvent event) {
        if (checkTarget(event.target) &&
            checkTypes(event.dataTransfer.types)) {
            event.preventDefault();
            event.dataTransfer.dropEffect = 'copy';
        }
    });
```

在以下代码中，`checkTypes` 函数仅接受通过 `dragstart` 事件监听器内部 `dataTransfer` 的 `setData` 方法指定的 `text/plain` 类型：

```
bool checkTypes(List<String> types) {
    return types.contains("text/plain");
}
```

你现在可以运行 Web 应用程序，将带有 `Drag me!` 文本的框拖动，并将其放置在带有 `Drop here` 文本的框内。让我们完善我们的应用程序并更改放置区域的文本。

## 完成放置

当用户释放鼠标时，拖放操作结束。如果这发生在被识别为有效放置目标的元素上，拖放 API 将在目标上触发 `drop` 事件。`drop` 事件的 `dataTransfer` 属性保存正在拖动的数据。为了检索拖动数据，我们将使用 `dataTransfer` 属性的 `getData` 方法，如下所示：

```
var dropTarget = querySelector("#sample_drop_id")
  ..onDragOver.listen((MouseEvent event) {
    if (checkTarget(event.target) &&
        checkTypes(event.dataTransfer.types)) {
      event.preventDefault();
      event.dataTransfer.dropEffect = 'copy';
    }
  })
  ..onDrop.listen((MouseEvent event) {
    Element dropTarget = event.target;
    dropTarget.innerHtml = event.dataTransfer.getData('text/plain');
    event.preventDefault();
  });
```

`getData` 方法将检索在调用 `setData` 方法时设置的字符串值。当从 `getData` 返回空字符串时，这意味着指定类型的数据不存在。在 `getData` 方法结束时，如果你已接受放置，则需要调用事件的 `preventDefault` 方法。以下是拖放操作的结果：

![](img/52c05b53a56abd53584e4fed738ed509_498_0.png)

## 完成拖动

最后，当拖动操作完成后，拖放 API 会在接收到 `dragstart` 事件的源元素上生成 `dragend` 事件。无论拖放操作的结果如何，API 都会生成该事件。`dropEffect` 属性的值可以告诉我们拖动操作的确切结果。如果 `dropEffect` 属性的值为 `none`，则拖动已取消；否则，指定的效果即为已执行的操作。拖放操作在 `dragend` 事件结束之后被视为完成。

## 地理位置 API

**地理位置** API 是一个用于定位信息的高级接口。它让你能够获知用户所在位置，并在用户移动时跟踪其位置。地理位置 API 与底层位置源无关，并且不关心 Web 浏览器如何确定位置。以下是常见的定位来源：

- GPS
- 网络 IP 地址
- RFID
- Wi-Fi
- 蓝牙 MAC 地址
- GSM/CDMA 基站 ID
- 用户输入

