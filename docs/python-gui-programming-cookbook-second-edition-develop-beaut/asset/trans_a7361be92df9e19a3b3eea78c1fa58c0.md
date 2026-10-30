

![](img/986b0dbf3f539f4f3baa82bd3003f04e_93_0.png)

## 如何工作…

注意我们的 `RadioButton(s)` 行突然出现在我们的 `ScrolledText` 控件中间！这绝对不是我们希望 GUI 看起来的样子！

![](img/986b0dbf3f539f4f3baa82bd3003f04e_93_1.png)

如果我们忘记显式指定行号，默认情况下 tkinter 将使用下一个可用的行。

我们还使用了 `columnspan` 属性来确保我们的控件不会仅限于一列。以下是我们确保 `ScrolledText` 控件跨越 GUI 所有列的方法：

```python
# Using a scrolled Text control
scroll_w = 30
scroll_h = 3
scr = scrolledtext.ScrolledText(mighty, width=scroll_w, height=scroll_h, wrap=tk.WORD)
scr.grid(column=0, row=2, sticky='WE', columnspan=3)                 # using columnspan
```

# 3

# 外观与风格自定义

在本章中，我们将使用 Python 3.6 及以上版本自定义我们的 GUI。我们将涵盖以下配方：

- 创建消息框 – 信息、警告和错误
- 如何创建独立的消息框
- 如何创建 tkinter 窗口表单的标题
- 更改主根窗口的图标
- 使用微调框控件
- 控件的浮雕、下沉和凸起外观
- 使用 Python 创建工具提示
- 向 GUI 添加进度条
- 如何使用画布控件

# 引言

在本章中，我们将通过更改其部分属性来自定义 GUI 中的一些控件。我们还将介绍 tkinter 为我们提供的几个新控件。

Creating tooltips using Python 配方将创建一个 ToolTip OOP 风格的类，它将成为我们迄今为止一直使用的单个 Python 模块的一部分。

以下是本章 Python 模块的概述：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_96_0.png)

# 创建消息框 – 信息、警告和错误

消息框是一个弹出窗口，向用户提供反馈。它可以是信息性的，提示潜在问题以及灾难性错误。

使用 Python 创建消息框非常容易。

# 准备工作

我们将向上一章 Creating tabbed widgets 配方中创建的 Help | About 菜单项添加功能。

代码来自 GUI_tabbed_all_widgets_both_tabs.py。大多数应用程序中点击 Help | About 菜单时对用户的典型反馈是信息性的。我们从此信息开始，然后改变设计模式以显示警告和错误。

# 如何操作…

在导入语句所在的模块顶部添加以下代码行：

```python
#===================
# imports
#===================
import tkinter as tk
from tkinter import ttk
from tkinter import scrolledtext
from tkinter import Menu
from tkinter import messagebox as msg
```

接下来，创建一个将显示消息框的回调函数。我们必须在将回调附加到菜单项的代码之上定位回调代码，因为这仍然是过程式而不是 OOP 代码。

在我们创建帮助菜单的行上方添加以下代码：

GUI_message_box.py

```python
# Display a Message Box
def _msgBox():
    msg.showinfo('Python Message Info Box', 'A Python GUI created using tkinter:\nThe year is 2017.')
# Add another Menu to the Menu Bar and an item
help_menu = Menu(menu_bar, tearoff=0)
help_menu.add_command(label="About", command=_msgBox)  # display messagebox when clicked
menu_bar.add_cascade(label="Help", menu=help_menu)
```

点击 Help | About 现在会弹出以下窗口：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_98_0.png)

让我们将此代码转换为警告消息框弹出窗口。注释掉上一行并添加以下代码：

```python
# Display a Message Box
def _msgBox():
#     msg.showinfo('Python Message Info Box', 'A Python GUI created using tkinter:'
#                 '\nThe year is 2017.')
    msg.showwarning('Python Message Warning Box', 'A Python GUI created using tkinter:'
                    '\n\nWarning: There might be a bug in this code.')
```

运行前面的代码现在将导致以下稍作修改的消息框：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_98_1.png)

显示错误消息框很简单，通常会警告用户严重问题。如前一个代码片段所示，注释掉上一行并添加以下代码，如下所示：

```python
# Display a Message Box
def _msgBox():
#     msg.showinfo('Python Message Info Box', 'A Python GUI created using tkinter:\nThe year is 2017.')
#     msg.showwarning('Python Message Warning Box', 'A Python GUI created using tkinter:'
#                     '\nWarning: There might be a bug in this code.')
    msg.showerror('Python Message Error Box', 'A Python GUI created using tkinter:'
                  '\nError: Houston ~ we DO have a serious PROBLEM!')
```

错误消息如下所示：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_99_0.png)

## 如何工作...

我们添加了另一个回调功能并将其作为委托附加以处理点击事件。现在，当我们点击 Help | About 菜单时，会发生一个动作。我们正在创建并显示最常见的弹出消息框对话框。它们是模态的，因此用户在点击 OK 按钮之前无法使用 GUI。

在第一个示例中，我们显示一个信息框，可以通过其左侧的图标看到。接下来，我们创建警告和错误消息框，它们会自动更改与弹出窗口关联的图标。我们要做的就是指定要显示哪个消息框。

有不同的消息框会显示不止一个 OK 按钮，我们可以根据用户的选择编程我们的响应。

以下是一个简单的示例，说明了此技术：

```python
# Display a Message Box
def _msgBox():
# msg.showinfo('Python Message Info Box', 'A Python GUI created using tkinter:\nThe year is 2017.')
# msg.showwarning('Python Message Warning Box', 'A Python GUI created using tkinter:\nWarning: There might be a bug in this code.')
# msg.showerror('Python Message Error Box', 'A Python GUI created using tkinter:\nError: Houston ~ we DO have a serious PROBLEM!')
answer = msg.askyesnocancel("Python Message Multi Choice Box", "Are you sure you really wish to do this?")
print(answer)
```

运行此 GUI 代码会产生一个弹出窗口，其用户响应可用于根据此事件驱动 GUI 循环的答案进行分支，通过将其保存在 answer 变量中：

GUI_message_box_yes_no_cancel.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_100_0.png)

使用 Eclipse 的控制台输出显示，点击 Yes 按钮会导致布尔值 True 被分配给 answer 变量：

```text
Search Console
C:\Eclipse_NEON_workspace\2nd Edition Python GUI Programming Cookbook\Ch03_Code\GUI_message_box_yes_no_cancel.py
True
```

例如，我们可以使用以下代码：

```python
if answer == True:
    <do something>
```

点击 No 返回 False，Cancel 返回 None。

# 如何创建独立的消息框

在本配方中，我们将把 tkinter 消息框创建为独立的顶级 GUI 窗口。

你会首先注意到，通过这样做，我们最终会得到一个额外的窗口，因此我们将探索隐藏此窗口的方法。

在上一配方中，我们通过主 GUI 表单中的 Help | About 菜单调用 tkinter 消息框。

那么，为什么我们希望创建独立的消息框呢？

一个原因是我们可能会自定义我们的消息框并在多个 GUI 中重用它们。无需将相同的代码复制并粘贴到我们设计的每个 Python GUI 中，我们可以将其从主 GUI 代码中提取出来。这可以创建一个小型可重用组件，我们随后可以将其导入到不同的 Python GUI 中。

# 准备工作

我们在上一配方 Creating message boxes - information, warning, and error 中已经创建了消息框的标题。我们将不重用上一配方的代码，而是使用很少的 Python 代码构建一个新的 GUI。

# 如何操作…

我们可以创建简单的消息框，如下所示：

```python
from tkinter import messagebox as msg
msg.showinfo('Python GUI created using tkinter:\nThe year is 2017')
```

