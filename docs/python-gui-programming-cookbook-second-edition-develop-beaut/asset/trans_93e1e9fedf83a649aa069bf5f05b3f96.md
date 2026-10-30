

```
```python
self.context = glcanvas.GLContext(self)

def OnPaint(self, event):
    dc = wx.PaintDC(self)
    # self.SetCurrent()          # commented out because:
    self.SetCurrent(self.context)  # We have to pass in a context
```
```

我们现在可以创建以下 GUI：

```
```python
import_OpenGL_cube_and_cone.py
```
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_366_0.png)

在经典版本的 wxPython 中，SetCurrent() 不需要上下文。以下是我们在网上搜索时可能会找到的一些代码：

```
```python
def OnPaint(self, event):
    dc = wx.PaintDC(self)
    self.SetCurrent()
    if not self.init:
        self.InitGL()
        self.init = True
    self.OnDraw()
```
```

使用 wxPython Phoenix 时，前述代码无法工作。我们可以在网上查找 Phoenix 的正确语法：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_367_0.png)

## How it works...

在本教程中，我们首次使用 PyOpenGL Python 绑定体验了 OpenGL。虽然 OpenGL 可以在真正的 3D 中创建真正令人惊叹的图像，但在途中我们遇到了一些挑战，然后找到了解决这些挑战的方案，使其得以工作。

我们正在用 Python 编程，创建 3D 图像！

## Our GUI in 3D!

在本教程中，我们将使用 wxPython 创建自己的 GUI。我们将重用 wxPython 演示示例中的一些代码，并将其精简为显示 3D OpenGL 所需的最小代码。

OpenGL 是一个非常庞大的库。我们不会详细介绍该库的说明。如果您想进一步研究 OpenGL，有大量书籍和在线文档可供查阅。它有自己的着色语言。

## Getting ready

阅读上一教程可能是本教程的良好准备。

## How to do it...

由于整个 Python 代码在这里有点长，我们只展示其中一小部分代码。

完整的代码可在网上获得，该 Python 模块名为 wxPython_OpenGL_GUI.py：

```
import wx
from wx import glcanvas
from OpenGL.GL import *
from OpenGL.GLUT import *

#------------------------------------------------------------
class CanvasBase(glcanvas.GLCanvas):
    def __init__(self, parent):
        glcanvas.GLCanvas.__init__(self, parent, -1)
        self.context = glcanvas.GLContext(self)
        self.init = False

        # Cube 3D start rotation
        self.last_X = self.x = 30
        self.last_Y = self.y = 30

        self.Bind(wx.EVT_SIZE, self.sizeCallback)
        self.Bind(wx.EVT_PAINT, self.paintCallback)
        self.Bind(wx.EVT_LEFT_DOWN, self.mouseDownCallback)
        self.Bind(wx.EVT_LEFT_UP, self.mouseUpCallback)
        self.Bind(wx.EVT_MOTION, self.mouseMotionCallback)

    def sizeCallback(self, event):
        wx.CallAfter(self.setViewport)
        event.Skip()

    def setViewport(self):
        self.size = self.GetClientSize()
        self.SetCurrent(self.context)
        glViewport(0, 0, self.size.width, self.size.height)

    def paintCallback(self, event):
        wx.PaintDC(self)
        self.SetCurrent(self.context)
        if not self.init:
```

```
```python
glNormal3f( 0.0, 0.0,-1.0)
glVertex3f(-0.5,-0.5,-0.5)

#============================================================
app = wx.App()
frame = wx.Frame(None, size=(300,230))
GUI(frame)
frame.Show()
app.MainLoop()
```
```

以下屏幕截图显示了我们的 wxPython GUI：wxPython_OpenGL_GUI.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_370_0.png)

当我们点击按钮控件时，会出现以下第二个窗口：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_371_0.png)

我们现在可以使用鼠标拖动立方体，以查看其六个面：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_371_1.png)

我们也可以最大化此窗口，坐标会缩放，并且可以在这个更大的窗口中旋转这个立方体！

![](img/986b0dbf3f539f4f3baa82bd3003f04e_372_0.png)

立方体也可以是一艘星际迷航的宇宙飞船！如果我们想开发这个，就必须成为该技术的资深程序员。

许多视频游戏都是使用 OpenGL 开发的。

## How it works...

我们首先创建了一个常规的 wxPython GUI，并在其上放置了一个按钮控件。点击此按钮会调用导入的 OpenGL 3D 库。使用的代码是 wxPython 演示示例的一部分，我们对其进行了轻微修改以使其与 Phoenix 兼容。

本教程将我们自己的 GUI 粘合到此库上。

OpenGL 是一个庞大而令人印象深刻的库。本教程展示了如何在 Python 中创建工作示例的思路。

通常，一个可工作的示例就是我们开始旅程所需的一切。

## Using bitmaps to make our GUI pretty

本教程的灵感来自于一个 wxPython IDE 构建框架，该框架在某个时间点曾经可以工作。我们将重用该项目提供的庞大代码量中的位图图像。

GitHub 仓库的 URL 为 https://github.com/reingart/gui2py。

我无法使用 Python 3.6 重现此 IDE。但它展示了可能性。如果您有兴趣开发拖放 IDE 框架，您可以通过让此代码在 Python 3.6 及更高版本上运行来帮助整个 Python Windows 开发者社区（并与 MS Visual Studio 竞争...）。

## Getting ready

我们将在本教程中继续使用 wxPython，因此阅读上一章的至少部分内容作为本教程的准备可能是有用的。

## How to do it...

在逆向工程 gui2py 代码并对代码进行其他更改后，我们可能会实现以下窗口控件，该控件显示漂亮的平铺背景：

```
wxPython_Wallpaper_simple.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_374_0.png)

当然，我们在重构上述网站的代码时丢失了很多控件，但它确实为我们提供了很酷的背景，点击 Quit 按钮仍然有效。

下一步是弄清楚如何将代码的有趣部分集成到我们自己的 GUI 中。我们通过将以下代码添加到上一教程中的 GUI 来实现这一点：

```
#-------------------------------------------------------------------------------
class GUI(wx.Panel):                 # Subclass wxPython Panel
    def __init__(self, parent):
        wx.Panel.__init__(self, parent)

        imageFile = 'Tile.bmp'
        self.bmp = wx.Bitmap(imageFile)
        # react to a resize event and redraw image
        parent.Bind(wx.EVT_SIZE, self.canvasCallback)
```

```
```python
def canvasCallback(self, event=None):
    # create the device context
    dc = wx.ClientDC(self)
    brushBMP = wx.Brush(self.bmp)
    dc.SetBrush(brushBMP)
    width, height = self.GetClientSize()
    dc.DrawRectangle(0, 0, width, height)
```
```

> > 我们必须绑定到 parent，而不是 self；否则，我们的位图将不会显示。

运行改进后的代码后，会将位图平铺为我们 GUI 的背景：

```
```text
wxPython_Wallpaper.py
```
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_375_0.png)

点击按钮仍然会调用我们的 OpenGL 3D 绘制，因此我们没有失去任何功能：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_376_0.png)

## How it works...

在本教程中，我们通过使用位图作为背景来增强了我们的 GUI。我们平铺了位图图像，并且在调整 GUI 窗口大小时，位图会自动调整自身以填满我们使用设备上下文绘制的画布的整个区域。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_376_1.png)

前述 wxPython 代码可以加载不同的图像文件格式。

## PyGLet transforms our GUI easier than PyOpenGL

在本教程中，我们将使用 PyGLet GUI 开发框架来创建我们的 GUI。

PyGLet 比 PyOpenGL 更易于使用，因为它自带自己的 GUI 事件循环，因此我们不需要使用 tkinter 或 wxPython 来创建 GUI。

## How to do it...

为了使用 PyGLet，我们首先必须安装这个第三方 Python 插件。

使用 pip 命令，我们可以轻松安装该库，成功安装后在我们的 site-packages Python 文件夹中看起来像这样：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_377_0.png)

