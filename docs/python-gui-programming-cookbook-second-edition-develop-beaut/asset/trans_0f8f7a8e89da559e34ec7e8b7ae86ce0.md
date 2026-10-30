

# 使用 wxPython 快速添加控件

在本节中，我们将重新创建本书前面用 tkinter 创建的 GUI，但这次我们将使用 wxPython 库。我们会看到使用 wxPython GUI 工具包创建我们自己的 Python GUI 是多么简单快捷。

我们不会重新创建前几章中实现的所有功能。例如，我们不会对 wxPython GUI 进行国际化，也不会将其连接到 MySQL 数据库。我们将重新创建 GUI 的视觉部分，并添加一些功能。

> 比较不同的库可以让我们选择使用哪些工具包来进行自己的 Python GUI 开发，而且我们可以在自己的 Python 代码中组合使用这些工具包中的多种。

## 准备工作

请确保你已安装 wxPython 模块，以便按照本节内容进行操作。

## 如何实现……

首先，像之前使用 tkinter 时那样创建我们的 Python OOP 类，但这次我们继承并扩展了 `wx.Frame` 类。为了清晰起见，我们不再将类命名为 OOP，而是将其重命名为 `MainFrame`。

> 在 wxPython 中，主 GUI 窗口被称为 Frame。

我们还创建了一个回调方法，当我们点击 **Exit** 菜单项时关闭 GUI，并声明一个浅灰色 `tuple` 作为 GUI 的背景颜色：

```
import wx
BACKGROUND_COLOR = (240, 240, 240, 255)

class MainFrame(wx.Frame):
    def __init__(self, *args, **kwargs):
        wx.Frame.__init__(self, *args, **kwargs)
        self.createWidgets()
        self.Show()
```

```
def exitGUI(self, event):      # callback
    self.Destroy()

def createWidgets(self):
    self.CreateStatusBar()      # wxPython built-in method
    self.createMenu()
    self.createNotebook()
```

然后，我们通过创建 wxPython Notebook 类的实例，为 GUI 添加一个选项卡控件，并将其作为我们自定义类 Widgets 的父级。

Notebook 类实例变量的父级是 wx.Panel：

```
#--------------------------------------------------
def createNotebook(self):
    panel = wx.Panel(self)
    notebook = wx.Notebook(panel)
    widgets = Widgets(notebook) # Custom class explained below
    notebook.AddPage(widgets, "Widgets")
    notebook.SetBackgroundColour(BACKGROUND_COLOR)
    # layout
    boxSizer = wx.BoxSizer()
    boxSizer.Add(notebook, 1, wx.EXPAND)
    panel.SetSizerAndFit(boxSizer)
```

在 wxPython 中，选项卡控件被命名为 Notebook，与 tkinter 中相同。

每个 Notebook 控件都需要有一个父级，并且为了在 wxPython 的 Notebook 中布局控件，我们使用不同类型的 sizer。

wxPython 的 sizer 是布局管理器，类似于 tkinter 的 grid 布局管理器。

接下来，我们向 Notebook 页面添加控件，方法是通过创建一个继承自 wx.Panel 的单独类：

```
class Widgets(wx.Panel):
    def __init__(self, parent):
        wx.Panel.__init__(self, parent)
        self.createWidgetsFrame()
        self.addWidgets()
        self.layoutWidgets()
```

## 使用 wxPython 库扩展我们的 GUI

我们按照 Python OOP 编程规范，将 GUI 代码分解为小方法来实现模块化，这使我们的代码保持可管理和易理解：

```
```python
#-----------------------------------------------
def createWidgetsFrame(self):
    self.panel = wx.Panel(self)
    staticBox = wx.StaticBox(self.panel, -1, "Widgets Frame")
    self.statBoxSizerV = wx.StaticBoxSizer(staticBox, wx.VERTICAL)
#-----------------------------------------------
def layoutWidgets(self):
    boxSizerV = wx.BoxSizer(wx.VERTICAL)
    boxSizerV.Add(self.statBoxSizerV, 1, wx.ALL)
    self.panel.SetSizer(boxSizerV)
    boxSizerV.SetSizeHints(self.panel)
#-----------------------------------------------
def addWidgets(self):
    self.addCheckBoxes()
    self.addRadioButtons()
    self.addStaticBoxWithLabels()
```
```

> 当使用 wxPython 的 `StaticBox` 控件时，为了成功布局它们，我们需要结合使用 `StaticBoxSizer` 和常规的 `BoxSizer`。wxPython 的 `StaticBox` 与 tkinter 的 `LabelFrame` 控件非常相似。

在 tkinter 中，将一个 `StaticBox` 嵌入另一个 `StaticBox` 中非常简单，但使用 wxPython 时则有点不太直观。下面代码片段展示了一种可行的方法：

```
```python
def addStaticBoxWithLabels(self):
    boxSizerH = wx.BoxSizer(wx.HORIZONTAL)
    staticBox = wx.StaticBox(self.panel, -1, "Labels within a Frame")
    staticBoxSizerV = wx.StaticBoxSizer(staticBox, wx.VERTICAL)
    boxSizerV = wx.BoxSizer(wx.VERTICAL)
    staticText1 = wx.StaticText(self.panel, -1, "Choose a number:")
    boxSizerV.Add(staticText1, 0, wx.ALL)
    staticText2 = wx.StaticText(self.panel, -1, "Label 2")
    boxSizerV.Add(staticText2, 0, wx.ALL)
    #-----------------------------------------------
    staticBoxSizerV.Add(boxSizerV, 0, wx.ALL)
    boxSizerH.Add(staticBoxSizerV)
    #-----------------------------------------------
    # boxSizerH.Add(wx.TextCtrl(self.panel))
    # Add local boxSizer to main frame
    self.statBoxSizerV.Add(boxSizerH, 1, wx.ALL)
```
```

首先，我们创建一个水平的 BoxSizer。接着，我们创建一个垂直的 StaticBoxSizer，因为我们希望在这个框架中垂直排列两个标签。

为了将另一个控件放在嵌入的 StaticBox 右侧，我们必须将嵌入的 StaticBox 及其子控件和下一个控件都分配给水平 BoxSizer，然后将这个现在同时包含嵌入的 StaticBox 和其他控件的 BoxSizer 分配给主 StaticBox。

这听起来是不是有点令人困惑？

你只需要亲自试验这些 sizer，才能熟悉它们的用法。从本节的代码开始，注释掉部分代码或修改一些 x 和 y 坐标来看看效果。

阅读官方 wxPython 文档来了解更多内容也是一个好主意。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_343_0.png)

重要的是知道在代码中应该向不同的 sizer 添加什么，才能实现我们想要的布局。

为了在第一个 StaticBox 下方创建第二个 StaticBox，我们创建单独的 StaticBoxSizer，并将它们分配给同一个面板：

```
class Widgets(wx.Panel):
    def __init__(self, parent):
        wx.Panel.__init__(self, parent)
        self.panel = wx.Panel(self)
        self.createWidgetsFrame()
        self.createManageFilesFrame()
        self.addWidgets()
        self.addFileWidgets()
        self.layoutWidgets()

    #------------------------------------------------------------
    def createWidgetsFrame(self):
        staticBox = wx.StaticBox(self.panel, -1, "Widgets Frame",
                                size=(285, -1))
        self.statBoxSizerV = wx.StaticBoxSizer(staticBox, wx.VERTICAL)
    #------------------------------------------------------------
    def createManageFilesFrame(self):
        staticBox = wx.StaticBox(self.panel, -1, "Manage Files",
                                size=(285, -1))
        self.statBoxSizerMgrV = wx.StaticBoxSizer(staticBox, wx.VERTICAL)
    #------------------------------------------------------------
    def layoutWidgets(self):
        boxSizerV = wx.BoxSizer( wx.VERTICAL )
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_345_0.png)

## 工作原理……

我们在几个类中设计和布局 wxPython GUI。

完成这些后，在 Python 模块的底部，我们创建 wxPython 应用的一个实例。接下来，我们实例化 wxPython GUI 代码。

之后，我们调用主 GUI 事件循环，该循环执行我们在此应用程序进程中运行的所有 Python 代码。这将显示我们的 wxPython GUI。

我们在创建 app 和调用其主事件循环之间放置的任何代码，都会成为我们的 wxPython GUI。也许需要一些时间才能真正适应 wxPython 库及其 API，但一旦我们理解了如何使用它，这个库真的很有趣，而且是构建我们自己的 Python GUI 的强大工具。还有一个可视化设计器工具可以与 wxPython 一起使用：
http://www.cae.tntech.edu/help/programming/wxdesigner-getting-started/view

本节使用了 OOP 来学习如何使用 wxPython GUI 工具包。

## 尝试将主 wxPython 应用嵌入主 tkinter 应用

现在我们已经使用 Python 内置的 tkinter 库以及 wxWidgets 库的 wxPython 封装创建了相同的 GUI，我们确实需要将使用这些技术创建的 GUI 结合起来。

wxPython 和 tkinter 库各有其优势。在 http://stackoverflow.com/ 等在线论坛中，我们经常看到诸如哪个更好、我应该使用哪个 GUI 工具包等问题。这似乎意味着我们必须做出二选一的决定。但我们不必做出这样的决定。

这样做的主要挑战之一是，每个 GUI 工具包都必须有自己的事件循环。

在本节中，我们将尝试从 tkinter GUI 中调用一个简单的 wxPython GUI，以将其嵌入其中。

## 准备工作

我们将复用第 1 章“创建 GUI 表单并添加控件”中“Combo Box 控件”一节构建的 tkinter GUI。

