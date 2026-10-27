

因为 Function 在 Dart 中是一个类型，所以函数可以很容易地作为参数传递。让我们来看一下带我们进入第一张幻灯片的按钮：

```
setButton("#btnFirst", startSlideShow);

void startSlideShow(MouseEvent event) {
  showFirstSlide();
}

void showFirstSlide() {
  showSlide(0);
}
```

事件处理程序不会直接更改幻灯片；这些操作由其他方法执行，而这些方法也可能由键盘等其他输入触发。

## 使用 Function 类型

SlideShowApp 构造函数使用了此功能：

```
Function qs = querySelector;
var controls = qs("#controls");
```

我觉得 querySelector 方法打起来有点长（尽管它描述了它的作用）。由于 Function 由类型组成，我们可以轻松创建一个简写版本。

构造函数花费了大量时间选择并将 HTML 元素分配给类的成员字段。这种方法的一个优点是，页面的 DOM 只会被查询一次，并且引用会被存储并重复使用。这对应用程序的性能很有好处，因为一旦应用程序运行起来，查询 DOM 可能需要更长的时间。

## 保持在边界内

使用 dart:math 包中的 min 和 max 函数，可以将索引保持在当前列表的范围内：

```
void showLastSlide() {
  currentSlideIndex = max(0, currentSlideShow.slides.length - 1);
  showSlide(currentSlideIndex);
}
void showNextSlide() {
```

```
currentSlideIndex =
min(currentSlideShow.slides.length - 1, ++currentSlideIndex);
showSlide(currentSlideIndex);
}
```

这些方便的函数可以节省大量 if 和 else if 比较，并使代码可读性提高很多。

## 使用滑块控件

滑块控件是 HTML5 标准中的另一个新控件。这将允许用户在演示文稿中滚动浏览幻灯片。

![](img/52c05b53a56abd53584e4fed738ed509_83_0.png)

这个控件是我个人最喜欢的控件之一，因为它非常直观，可以为用户提供非常交互式的反馈。在早期一代网络浏览器的原始表单控件中，似乎遗漏了它。即使是清晰且被广泛接受的功能，HTML 规范也可能需要很长时间才能通过委员会并进入日常浏览器！

```
<input type="range" id="rngSlides" value="0"/>
```

该控件有一个 onChange 事件，在 SlideShowApp 构造函数中为其添加了监听器：

```
rangeSlidepos.onChange.listen(moveToSlide);rangeSlidepos.onChange
.listen(moveToSlide);
```

该控件通过简单的字符串值提供其数据，该值可以通过 int.parse 方法转换为整数，以便用作演示文稿幻灯片列表中的索引：

```
void moveToSlide(Event event) {
  currentSlideIndex = int.parse(rangeSlidePos.value);
  showSlide(currentSlideIndex);
}
```

滑块控件必须与幻灯片显示的任何其他更改、导航的使用或幻灯片数量的更改保持同步。例如，用户可以使用滑块到达演示文稿的一般区域，然后使用“上一个”和“下一个”按钮进行调整：

```
void updateRangeControl() {
  rangeSlidepos
  ..min = "0"
  ..max = (currentSlideShow.slides.length - 1).toString();
}
```

此方法在幻灯片数量更改时调用，并且与处理大多数 HTML 元素一样，需要将要设置的值转换为字符串。

## 响应键盘事件

使用键盘，特别是箭头（光标）键，是在预览模式下浏览演示文稿幻灯片的自然方式。这是在 SlideShowApp 构造函数中完成的。

> 在 Dart Web 应用程序中，dart:html 包允许从任何类或函数直接访问全局 window 对象。

用于输入演示文稿源的 Textarea 也会响应箭头键，因此需要检查它当前是否正在使用。document 上的 activeElement 属性将给出具有焦点的控件的引用。此引用可以与存储在 presEditor 字段中的 Textarea 进行比较，以便决定是否对按键做出反应。

| 键 | 事件代码 | 操作 |
|---|---|---|
| 左箭头 | 37 | 后退一页幻灯片 |
| 上箭头 | 38 | 转到第一张幻灯片 |
| 右箭头 | 39 | 转到下一张幻灯片 |
| 下箭头 | 40 | 转到最后一张幻灯片 |

## 两次监听事件流

Dart 中的事件系统实现为流。其优点之一是事件可以很容易地让多个实体监听该类。

例如，在 Web 应用程序中，这很有用，因为某些键盘按下在一种上下文中有效，而在另一种上下文中无效。listen 方法是一个添加操作（累积的），因此帮助的按键可以单独实现。这允许模块化方法，有助于重用，因为处理程序可以专门化并根据需要添加：

```
window.onKeyUp.listen((KeyboardEvent e) {
  print(e);

  //Check the editor does not have focus.
  if (presEditor != document.activeElement) {
    DivElement helpBox = qs("#helpKeyboardShortcuts");
    if (e.keyCode == 191) {
      if (helpBox.style.visibility == "visible") {
        helpBox.style.visibility = "hidden";
      } else {
        helpBox.style.visibility = "visible";
      }
    }
  }
});
```

例如，在游戏中，一组常见的事件处理可能适用于标题和介绍屏幕，而实际游戏屏幕可以包含作为超集的额外事件处理。这可以通过向相关事件流添加和删除处理程序来实现。

## 更改颜色

HTML5 为浏览器提供了功能齐全的颜色选择器（通常，浏览器使用操作系统的本机颜色选择器）。这将用于允许用户设置编辑器应用程序本身的背景颜色：

默认值为 `index.html` 页面中设置如下：

![](img/52c05b53a56abd53584e4fed738ed509_88_0.png)

`DateInputElement` 类的 `valueAsDate` 属性提供 `Date` 对象，可以将其添加到文本区域：

```
```javascript
void insertDate(Event event) {
    DateInputElement datePicker = querySelector("#selDate");
    if (datePicker.valueAsDate != null) presEditor.value =
    presEditor.value +
    datePicker.valueAsDate.toLocal().toString();
}
```
```

在这种情况下，使用 `toLocal` 方法来获取格式化为月、日和年格式的字符串。

## 计时演示

演讲者希望遵守其分配的时间段。我们将在编辑器中包含一个计时器以帮助排练。

## 介绍 Stopwatch 类

Stopwatch 类（来自 `dart:core`）提供了此功能所需的大部分功能，如以下小型命令行应用程序所示：

```
```dart
main() {
    Stopwatch sw = new Stopwatch();
    sw.start();
    print(sw.elapsed);
    sw.stop();
    print(sw.elapsed);
}
```
```

elapsed 属性可以在任何时候检查以给出当前持续时间。这是一个非常有用的类，例如，它可用于比较不同的函数以查看哪个最快。

## 实现演示计时器

时钟将通过 `toggleTimer` 方法处理的单个按钮停止和启动。重复计时器将更新屏幕上的持续时间文本，如下所示：

![](img/52c05b53a56abd53584e4fed738ed509_89_0.png)

如果计时器正在运行，则更新 `Timer` 和字段 `slidesTime` 中的 `Stopwatch` 将被停止。无需更新显示，因为用户需要看到最终时间：

```
```dart
void toggleTimer(Event event) {
  if (slidesTime.isRunning) {
    slidesTime.stop();
    updateTimer.cancel();
  } else {
    updateTimer = new Timer.periodic(new Duration(seconds: 1),
      (timer) {
        String seconds = (slidesTime.elapsed.inSeconds %
        60).toString();
        seconds = seconds.padLeft(2, "0");
        timerDisplay.text =
          "${slidesTime.elapsed.inMinutes}:$seconds";
      });

    slidesTime
      ..reset()
      ..start();
  }
}
```
```

Stopwatch 类提供了以分钟和秒检索经过时间的属性。为了将其格式化为分钟和秒，秒部分是使用模除运算符 % 确定的，并使用字符串函数 `padLeft` 进行填充。

Dart 的字符串插值功能用于构建最终字符串，并且由于正在访问 elapsed 和 `inMinutes` 属性，因此需要 `{}` 括号，以便返回单个值。

