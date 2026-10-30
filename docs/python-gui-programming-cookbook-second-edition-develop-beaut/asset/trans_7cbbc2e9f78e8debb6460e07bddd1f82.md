

这将产生以下两个窗口：

```
GUI_independent_msg.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_102_0.png)

这看起来并非我们所期望的。现在，我们有两个窗口：一个是多余的，另一个则将文本显示成了标题。

哎呀！

让我们现在来解决这个问题。我们可以修改 Python 代码，添加一个单引号或双引号，后面再跟一个逗号：

```python
from tkinter import messagebox as msg
msg.showinfo('', 'Python GUI created using tkinter:\nThe year is 2017')
```

现在，我们没有标题了，但文本如预期那样显示在了弹出窗口中：

```
GUI_independent_msg_info.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_102_1.png)

第一个参数是标题，第二个参数是弹出消息框中显示的文本。通过添加一对空的单引号或双引号，后面再跟一个逗号，我们就可以将文本从标题移到弹出消息框中。

我们仍然需要一个标题，而且肯定希望去掉这个多余的第二窗口。第二个窗口是由 Windows 事件循环引起的。我们可以通过抑制它来摆脱它。

添加以下代码：

```python
from tkinter import messagebox as msg
from tkinter import Tk
root = Tk()
root.withdraw()
msg.showinfo('', 'Python GUI created using tkinter:\nThe year is 2017')
```

现在，我们只剩一个窗口了。`withdraw()` 函数会移除我们不想让其四处漂浮的调试窗口：

```
GUI_independent_msg_one_window.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_103_0.png)

为了添加标题，我们只需在空的第一个参数中放入某个字符串即可。

## 外观与感觉定制

例如，考虑以下代码片段：

```python
from tkinter import messagebox as msg
from tkinter import Tk
root = Tk()
root.withdraw()
msg.showinfo('This is a Title', 'Python GUI created using tkinter:\nThe year is 2017')
```

现在，我们的对话框有了标题，如下面的截图所示：

```
GUI_independent_msg_one_window_title.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_104_0.png)

## 工作原理……

我们向消息框的 tkinter 构造函数传递更多参数，以便为窗口窗体添加标题，并在消息框中显示文本，而不是将其显示为标题。这是因为我们所传递参数的位置。如果我们省略空的单引号或双引号，那么消息框控件就会将第一个参数位置作为标题，而不是要在消息框内显示的文本。通过传递一个空引号后跟一个逗号，我们改变了消息框显示我们传递给函数的文本的位置。

我们通过在根窗口上调用 `withdraw()` 方法来抑制 tkinter 消息框控件自动创建的第二个弹出窗口。

## 如何创建 tkinter 窗口窗体的标题

更改 tkinter 主根窗口标题的原理与我们在上一个教程中讨论的“如何创建独立消息框”相同。我们只需将一个字符串作为第一个参数传递给控件的构造函数即可。

## 准备

我们不创建弹出对话框窗口，而是创建主根窗口并为其指定标题。

## 操作步骤……

以下代码创建主窗口并为其添加标题。我们在前面的教程中已经做过类似操作，例如在第 2 章“布局管理”的“创建选项卡控件”中。这里我们只关注 GUI 的这一方面：

```python
import tkinter as tk
win = tk.Tk()      # Create instance
win.title("Python GUI")  # Add a title
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_105_0.png)

## 工作原理……

这通过使用 tkinter 内置的 `title` 属性来设置主根窗口的标题。在我们创建 `Tk()` 实例后，就可以使用所有 tkinter 内置属性来自定义我们的 GUI。

## 更改主根窗口的图标

自定义 GUI 的一种方式是给它一个不同于 tkinter 默认图标的图标。下面是我们如何做到这一点的。

## 准备

我们正在改进第 2 章“布局管理”中“创建选项卡控件”教程中的 GUI。我们将使用一个随 Python 一同提供的图标，但您也可以使用任何您觉得有用的图标。请确保代码中包含了图标的完整路径，否则可能会出错。

## 操作步骤……

在本示例中，我将从安装 Python 3.6 的目录中复制了图标到代码所在的同一个文件夹中。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_106_0.png)

将以下代码放在主事件循环上方的某个位置：

```python
# Change the main window's icon
win.iconbitmap('pyc.ico')
```

注意 GUI 左上角默认的羽毛图标是如何变化的：

```
GUI_icon.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_107_0.png)

## 工作原理……

这是 tkinter 随附的另一个属性，它随 Python 3.6 及更高版本一起提供。我们使用 `iconbitmap` 属性，通过传入一个图标的相对路径来更改主根窗口窗体的图标。这会覆盖 tkinter 的默认图标，将其替换为我们选择的图标。

## 使用微调框控件

在本教程中，我们将使用一个 Spinbox 控件，并将键盘上的回车键绑定到我们某个控件上。

## 准备

我们将使用带选项卡的 GUI，并在 ScrolledText 控件上方添加一个 Spinbox 控件。这只需要我们将 ScrolledText 的行数值加一，然后将新的 Spinbox 控件插入到 Entry 控件上方的那一行。

## 操作步骤……

首先，我们添加 Spinbox 控件。将以下代码放在 ScrolledText 控件上方：

```python
# Adding a Spinbox widget
spin = Spinbox(mighty, from_=0, to=10)
spin.grid(column=0, row=2)
```

这将修改我们的 GUI 如下：

```
GUI_spinbox.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_108_0.png)

接下来，我们将减小 Spinbox 控件的大小：

```python
spin = Spinbox(mighty, from_=0, to=10, width=5)
```

运行上述代码会得到以下 GUI：

```
GUI_spinbox_small.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_108_1.png)

接下来，我们添加另一个属性来进一步定制我们的控件；`bd` 是 `borderwidth` 属性的简写：

```python
spin = Spinbox(mighty, from_=0, to=10, width=5, bd=8)
```

运行上述代码会得到以下 GUI：

```
GUI_spinbox_small_bd.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_109_0.png)

在这里，我们通过创建回调函数并将其链接到控件来为控件添加功能。

这会将 Spinbox 的选择打印到 ScrolledText 中，并同时打印到标准输出。变量 `scrol` 是我们对 ScrolledText 控件的引用：

```python
# Spinbox callback
def _spin():
    value = spin.get()
    print(value)
    scrol.insert(tk.INSERT, value + '\n')

spin = Spinbox(mighty, from_=0, to=10, width=5, bd=8,
                command=_spin)
```

运行上述代码会得到以下 GUI：

```
GUI_spinbox_small_bd_scrol.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_110_0.png)

除了使用范围，我们也可以指定一组值：

```python
# Adding a Spinbox widget using a set of values
spin = Spinbox(mighty, values=(1, 2, 4, 42, 100), width=5, bd=8,
               command=spin)
spin.grid(column=0, row=2)
```

这将创建以下 GUI 输出：

```
GUI_spinbox_small_bd_scrol_values.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_110_1.png)

## 工作原理……

请注意，在第一张截图中，我们新的 Spinbox 控件默认宽度为 20，这将该列中所有控件的列宽都推了出去。这不是我们想要的。

我们给控件指定了从 0 到 10 的范围。

在第二张截图中，我们减小了 Spinbox 控件的宽度，使其在列中居中对齐。

在第三张截图中，我们添加了 Spinbox 的 `borderwidth` 属性，这使整个 Spinbox 看起来不再扁平，而是具有了三维效果。

在第四张截图中，我们添加了一个回调函数，用于在 ScrolledText 控件中显示所选数字，并将其打印到标准输出流中。我们添加了 `\n` 以便在新行上打印。注意默认值不会被打印。只有当我们点击控件时，回调函数才会被调用。通过将默认值 0 向下箭头点击，我们可以打印出 0 值。

最后，我们将可用值限制为一组硬编码的值。这也可以从数据源（例如文本或 XML 文件）中读取。

## 控件的浮雕、凹陷和凸起外观

我们可以通过使用一个属性来控制 Spinbox 控件的外观，使其呈现不同的凹陷或凸起格式。

## 准备

我们将再添加一个 Spinbox 控件，以演示可用的控件外观，使用 Spinbox 控件的 `relief` 属性。

