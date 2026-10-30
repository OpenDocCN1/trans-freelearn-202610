

## 如何实现...

我们将从一个简单的 tkinter GUI 开始，如下所示：

Embed_wxPython.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_347_0.png)

接下来，我们将尝试调用一个简单的 wxPython GUI，它是在本章之前的一个食谱中创建的。

以下是在一个简单的非 OOP 方式下执行此操作的完整代码：

```
#=============================================================
import tkinter as tk
from tkinter import ttk, scrolledtext

win = tk.Tk()
win.title("Python GUI")
aLabel = ttk.Label(win, text="A Label")
aLabel.grid(column=0, row=0)
ttk.Label(win, text="Enter a name:").grid(column=0, row=0)
name = tk.StringVar()
nameEntered = ttk.Entry(win, width=12, textvariable=name)
nameEntered.grid(column=0, row=1)
ttk.Label(win, text="Choose a number:").grid(column=1, row=0)
number = tk.StringVar()
numberChosen = ttk.Combobox(win, width=12, textvariable=number)
numberChosen['values'] = (1, 2, 4, 42, 100)
numberChosen.grid(column=1, row=1)
numberChosen.current(0)
scrolW = 30
scrolH = 3
scr = scrolledtext.ScrolledText(win, width=scrolW, height=scrolH, wrap=tk.WORD)
scr.grid(column=0, sticky='WE', columnspan=3)
nameEntered.focus()
#===============================================
def wxPythonApp():
    import wx
    app = wx.App()
    frame = wx.Frame(None, -1, "wxPython GUI", size=(200,150))
    frame.SetBackgroundColour('white')
    frame.CreateStatusBar()
    menu= wx.Menu()
    menu.Append(wx.ID_ABOUT, "About", "wxPython GUI")
    menuBar = wx.MenuBar()
    menuBar.Append(menu,"File")
    frame.SetMenuBar(menuBar)
    frame.Show()
    app.MainLoop()

action = ttk.Button(win, text="Call wxPython GUI", command=wxPythonApp)
action.grid(column=2, row=1)

#===================
# Start GUI
#===================
win.mainloop()
```

运行上述代码后，点击 tkinter 按钮控件，将从 tkinter GUI 启动一个 wxPython GUI：

Embed_wxPython.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_348_0.png)

## 工作原理...

重要的是，我们将整个 wxPython 代码放入其自己的函数中，命名为 def wxPythonApp()。

在按钮点击事件的回调函数中，我们只需调用这段代码。

> > 需要注意的一点是，我们必须先关闭 wxPython GUI，然后才能继续使用 tkinter GUI。

## 尝试将我们的 tkinter GUI 代码嵌入到 wxPython 中

在本食谱中，我们将沿着与之前食谱相反的方向，尝试在 wxPython GUI 中调用我们的 tkinter GUI 代码。

### 准备工作

我们将复用本章之前食谱中创建的一些 wxPython GUI 代码。

### 如何实现...

我们将从一个简单的 wxPython GUI 开始，如下所示：

`Embed_tkinter.py`

![](img/986b0dbf3f539f4f3baa82bd3003f04e_349_0.png)

接下来，我们将尝试调用一个简单的 tkinter GUI。

运行上述代码后，点击 wxPython 按钮控件，将从 wxPython GUI 启动一个 tkinter GUI。然后，我们可以在 tkinter 文本框中输入文本，并点击其按钮，按钮文本将更新为该名称：

`Embed_tkinter.py`

![](img/986b0dbf3f539f4f3baa82bd3003f04e_351_0.png)

启动 tkinter 事件循环后，当 tkinter GUI 正在运行时，我们仍然可以向 TextCtrl 控件中输入文本，因此 wxPython GUI 仍然保持响应。

在之前的食谱中，我们必须先关闭 wxPython GUI，然后才能使用 tkinter GUI。如果我们想将这两种 Python GUI 技术结合起来，了解这一差异有助于我们的设计决策。

我们还可以通过多次点击 wxPython GUI 按钮来创建多个 tkinter GUI 实例。但是，只要任何 tkinter GUI 仍在运行，我们就无法关闭 wxPython GUI。我们必须先关闭它们：

`Embed_tkinter.py`

![](img/986b0dbf3f539f4f3baa82bd3003f04e_351_1.png)

### 工作原理...

在本食谱中，我们沿与之前食谱相反的方向：首先使用 wxPython 创建 GUI，然后从其中创建多个使用 tkinter 构建的 GUI 实例。

当一个或多个 tkinter GUI 运行时，wxPython GUI 仍然保持响应。然而，点击 tkinter 按钮只会更新第一个实例中的按钮文本。

## 使用 Python 控制两个不同的 GUI 框架

在本食谱中，我们将探讨从 Python 控制 tkinter 和 wxPython GUI 框架的方法。我们已经在前面的一章“线程与网络”中使用过 Python 的 threading 模块来保持 GUI 的响应性，因此这里我们将尝试使用相同的方法。

我们会发现，事情并不总是按照直觉的方式运作。

然而，我们将改进我们的 tkinter GUI，使其在从内部调用 wxPython GUI 实例时不会失去响应。

### 准备工作

本食谱将扩展本章之前的食谱“尝试将主 wxPython 应用嵌入主 tkinter 应用”，在该食谱中，我们尝试将主 wxPython GUI 嵌入到 tkinter GUI 中。

### 如何实现...

当我们从 tkinter GUI 创建 wxPython GUI 实例时，在关闭这个 wxPython GUI 实例之前，我们无法再使用 tkinter GUI 控件。现在让我们对此进行改进。

我们的第一次尝试可能是从 tkinter 按钮回调函数中使用线程。

例如，我们的代码可能如下所示：

```
def wxPythonApp():
    import wx
    app = wx.App()
    frame = wx.Frame(None, -1, "wxPython GUI", size=(200,150))
    frame.SetBackgroundColour('white')
    frame.CreateStatusBar()
    menu= wx.Menu()
    menu.Append(wx.ID_ABOUT, "About", "wxPython GUI")
    menuBar = wx.MenuBar()
    menuBar.Append(menu,"File")
    frame.SetMenuBar(menuBar)
    frame.Show()
    app.MainLoop()

def tryRunInThread():
    runT = Thread(target=wxPythonApp)
    runT.setDaemon(True)
    runT.start()
    print(runT)
    print('createThread() :', runT.isAlive())

action = ttk.Button(win, text="Call wxPython GUI", command=tryRunInThread)
```

起初，这似乎是有效的，这也符合直觉，因为 tkinter 控件不再被禁用，我们可以通过点击按钮创建多个 wxPython GUI 实例。我们还可以在 wxPython GUI 中输入文本，并选择其他 tkinter 控件：

Control_Frameworks_NOT_working.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_353_0.png)

### 工作原理...

我们首先尝试在一个线程中运行整个 wxPython GUI 应用程序，但这没有用，因为 wxPython 的主事件循环期望成为应用程序的主线程。

我们通过只在一个线程中运行 wxPython 的 `app.MainLoop` 找到了解决方法，使其误以为自己是主线程。

这种方法的一个副作用是，我们无法再单独关闭所有 wxPython GUI 实例。其中至少有一个实例只在我们关闭创建线程为守护线程的 wxPython GUI 时才会关闭。您可以通过点击 **调用 wxPython GUI** 按钮一次或多次，然后尝试关闭所有已创建的 wxPython 窗口窗体来测试这一点。除非我们先关闭调用方的 tkinter GUI，否则我们无法关闭最后一个 wxPython GUI！

我不太确定为什么会这样。凭直觉，人们可能期望能够关闭所有守护线程，而不必等待创建它们的主线程先关闭。

这可能与主线程仍在运行时引用计数器尚未归零有关。

从实际角度来看，这就是当前的工作方式。

## 在两个连接的 GUI 之间通信

在之前的食谱中，我们找到了将 wxPython GUI 与 tkinter GUI 连接的方法，可以从一个调用另一个，反之亦然。

虽然两个 GUI 同时成功运行，但它们并没有真正相互通信，因为它们只是相互启动。

在本食谱中，我们将探讨让两个 GUI 相互对话的方法。

### 准备工作

阅读前面某个食谱可能对本食谱有很好的帮助。

在本食谱中，我们将使用相对于前一个食谱稍作修改的 GUI 代码，但大多数基本的 GUI 构建代码是相同的。

### 如何实现...

在之前的食谱中，我们的主要挑战之一是如何组合两种 GUI 技术，而它们都被设计为应用程序唯一的 GUI 工具包。我们找到了各种简单的方法来组合它们。

我们将再次从 tkinter GUI 的主事件循环启动 wxPython GUI，并在其自己的线程中启动 wxPython GUI，该线程在 Python.exe 进程内运行。

为此，我们将使用一个共享的全局 multiprocessing Python 队列。

虽然通常最好避免使用全局数据，但在本食谱中，这是一个实用的解决方案，而且 Python 全局变量实际上只在其声明的模块中是全局的。

下面是使两个 GUI 在一定程度上相互通信的 Python 代码。为了节省篇幅，这不是纯 OOP 代码。我们也没有展示所有控件的创建代码。这些代码与之前的食谱相同：

```
# Communicate.py
import tkinter as tk
from tkinter import ttk
from threading import Thread

win = tk.Tk()
win.title("Python GUI")

from queue import Queue
sharedQueue = Queue()
dataInQueue = False

def putDataIntoQueue(data):
    global dataInQueue
    dataInQueue = True
    sharedQueue.put(data)

def readDataFromQueue():
```

