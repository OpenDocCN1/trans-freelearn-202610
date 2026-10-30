

```
global dataInQueue
    dataInQueue = False
    return sharedQueue.get()
#================================================================
import wx
class GUI(wx.Panel):
    def __init__(self, parent):
        wx.Panel.__init__(self, parent)
        parent.CreateStatusBar()
        button = wx.Button(self, label="Print", pos=(0,60))
        self.Bind(wx.EVT_BUTTON, self.writeToSharedQueue, button)

    #------------------------------------------------------------
    def writeToSharedQueue(self, event):
        self.textBox.AppendText("The Print Button has been clicked!\n")
        putDataIntoQueue('Hi from wxPython via Shared Queue.\n')
        if dataInQueue:
            data = readDataFromQueue()
            self.textBox.AppendText(data)
            text.insert('0.0', data) # insert data into tkinter GUI

#================================================================
def wxPythonApp():
    app = wx.App()
    frame = wx.Frame(None, ,
                    size=(300,180))
    GUI(frame)
    frame.Show()
    runT = Thread(target=app.MainLoop)
    runT.setDaemon(True)
    runT.start()
    print(runT)
    print('createThread():', runT.isAlive())
#================================================================
action = ttk.Button(win, text="Call wxPython GUI", command=wxPythonApp)
action.grid(column=2, row=1)

#==================
# Start GUI
#==================
win.mainloop()
```

## 使用 wxPython 库扩展我们的 GUI

运行前面的代码首先创建程序的 tkinter 部分，当我们点击此 GUI 中的按钮时，它会运行 wxPython GUI。两者像之前一样同时运行，但这次两个 GUI 之间有额外的通信层：

```
Communicate.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_358_0.png)

前面的截图中左侧显示了 tkinter GUI，通过点击 **Call wxPython GUI** 按钮，我们会调用一个 wxPython GUI 实例。我们可以通过多次点击按钮来创建多个实例。

> 所有创建的 GUI 都保持响应。它们不会崩溃也不会冻结。

在任何 wxPython GUI 实例上点击 **Print** 按钮会向其自己的 TextCtrl 小部件写入一句话，然后再向自身以及 tkinter GUI 写入另一行。你需要向上滚动才能在 wxPython GUI 中看到第一句话。

> 这样工作的方式是使用模块级队列和 tkinter Text 小部件。

一个需要注意的重要元素是，我们创建了一个线程来运行 wxPython app.MainLoop，就像我们在上一节食谱中所做的那样：

```
def wxPythonApp():
    app = wx.App()
    frame = wx.Frame(None, ,
                    size=(300,180))
    GUI(frame)
    frame.Show()
    runT = Thread(target=app.MainLoop)
    runT.setDaemon(True)
    runT.start()
```

我们创建一个继承自 wx.Panel 的类并命名为 GUI，然后在前面的代码中实例化该类的一个实例。

我们在此类中创建一个按钮点击事件回调方法，该方法随后调用在其上方编写的过程代码。正因为如此，该类可以访问函数并写入共享队列：

```
#----------------------------------------------------------------
def writeToSharedQueue(self, event):
    self.textBox.AppendText("The Print Button has been clicked\n")
    putDataIntoQueue('Hi from wxPython via Shared Queue.\n')
    if dataInQueue:
        data = readDataFromQueue()
        self.textBox.AppendText(data)
        text.insert('0.0', data) # insert data into tkinter
```

我们在前面的方法中首先检查数据是否已放置在共享队列中，如果是，则将公共数据打印到两个 GUI。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_359_0.png)

putDataIntoQueue() 行将数据放入队列，而 readDataFromQueue() 则将其读回，保存在 data 变量中。text.insert('0.0', data) 是将此数据从 Print 按钮的 wxPython 回调方法写入 tkinter GUI 的行。

以下是在代码中被调用并使其工作的过程函数（不是方法，因为它们未绑定）：

```
from multiprocessing import Queue
sharedQueue = Queue()
dataInQueue = False

def putDataIntoQueue(data):
    global dataInQueue
    dataInQueue = True
```

## 10

## 使用 PyOpenGL 和 PyGLet 创建惊人的 3D GUI

在本章中，我们将创建显示真实三维图像的惊人 Python GUI，这些图像可以围绕自身旋转，以便我们从各个角度观察它们。我们将涵盖以下食谱：

- PyOpenGL 转换我们的 GUI
- 我们的 GUI 在 3D 中！
- 使用位图使我们的 GUI 更漂亮
- PyGLet 比 PyOpenGL 更容易转换我们的 GUI
- 我们的 GUI 呈现惊人的色彩
- OpenGL 动画
- 使用 tkinter 创建幻灯片放映

## 简介

在本章中，我们将通过赋予其真正的三维功能来转换我们的 GUI。我们将使用两个 Python 第三方包。PyOpenGL 是 OpenGL 标准的 Python 绑定，OpenGL 是一个图形库，已内置于所有主要操作系统中。这使得生成的部件具有原生的外观和感觉。

PyGLet 是另一个 OpenGL 库的 Python 绑定，但它也可以创建 GUI 应用程序，这使得使用 PyGLet 编码比使用 PyOpenGL 更容易。

以下是本章的 Python 模块概述：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_362_0.png)

## PyOpenGL 转换我们的 GUI

在本节食谱中，我们将成功创建一个导入 PyOpenGL 模块并实际工作的 Python GUI！为了做到这一点，我们将克服一些初始挑战。

本食谱将展示一种经过验证可行的方法。如果你自己尝试并遇到困难，请记住托马斯·A·爱迪生的名言。

托马斯·爱迪生，灯泡的发明者，回答了一位记者关于爱迪生失败的问题。爱迪生回答说：“我没有失败。我只是找到了 10,000 种行不通的方法。”

首先，我们必须安装 PyOpenGL 扩展模块。成功安装与我们操作系统架构匹配的 PyOpenGL 模块后，我们将创建一些示例代码。

### 准备工作

我们将安装 PyOpenGL 包。在本书中，我们使用的是 Windows 10 64 位操作系统和 Python 3.6。下面的下载截图适用于此配置。

我们还将使用 wxPython。如果你没有安装 wxPython，可以阅读上一章中的一些食谱，了解如何安装 wxPython 以及如何使用此 GUI 框架。

我们使用的是 wxPython Phoenix 版本，这是最新版本，未来将取代原始的经典 wxPython 版本。

### 如何操作...

为了使用 PyOpenGL，我们必须先安装它。以下 URL 是官方 Python 包安装网站：

```
https://pypi.python.org/pypi/PyOpenGL/3.0.2.
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_364_0.png)

这似乎是正确的安装，但事实证明它与 Windows 10 64 位操作系统和 Python 3.6 64 位不兼容。

在上一章的一个食谱中提到了一个更好的 Python 安装包查找位置。你可能已经很熟悉了。URL 为 http://www.lfd.uci.edu/~gohlke/pythonlibs/。

我们必须下载同时匹配我们操作系统和 Python 版本的包。它带有新的 .whl 格式。如果你使用的是较旧版本的 Python，你必须先安装 Python wheel 包：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_364_1.png)

如何安装 Python wheel 包在第 5 章 Matplotlib Charts 中，Installing Matplotlib using pip with whl extension 食谱中有描述。

使用 pip 命令通过 PyOpenGL-3.1.1-cp36-cp36m-win_amd64.whl 文件安装 PyOpenGL 既成功又安装了我们所需的所有 64 位模块。

将 <your full path> 替换为你下载 .whl 安装程序的完整路径：

```
pip install <your full path> PyOpenGL-3.1.1-cp36-cp36m-win_amd64.whl
```

现在当我们尝试导入一些 PyOpenGL 模块时，它可以工作，如此代码示例所示：

```
import wx
from wx import glcanvas
from OpenGL.GL import *
from OpenGL.GLUT import *
```

这段代码所做的只是导入几个 OpenGL Python 模块，以及导入 wxPython。它不做其他任何事情，但当我们运行 Python 模块时，我们不会收到任何错误。

这证明我们已成功将 OpenGL 绑定到 Python。

现在我们的开发环境已成功设置，我们可以使用 wxPython 尝试一下。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_365_0.png)

许多在线示例仅限于使用 Python 2 以及使用经典版本的 wxPython。我们使用的是 Python 3.6 和 Phoenix。

基于 wxPython 演示示例的代码创建了一个工作的 3D 立方体。相比之下，运行锥体示例不起作用，但该示例让我们走上了正确的轨道。

以下是 URL：http://wiki.wxpython.org/GLCanvas%20update

以下是代码的一些修改：

```
import wx
from wx import glcanvas
from OpenGL.GL import *
from OpenGL.GLUT import *
```

```
class MyCanvasBase(glcanvas.GLCanvas):
    def __init__(self, parent):
        glcanvas.GLCanvas.__init__(self, parent, -1)
        # This context was missing from the original code
```

