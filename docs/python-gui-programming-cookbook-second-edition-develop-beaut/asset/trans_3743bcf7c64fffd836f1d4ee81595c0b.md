

![](img/986b0dbf3f539f4f3baa82bd3003f04e_387_0.png)

接下来，我们将通过让红色方块在窗口中移动并在每个角落反弹来为其制作动画。为此，我们首先创建一些模块级全局变量：

```
OpenGL_SuperBible_Animation.py
```

```
```python
from OpenGL.GLUT import *
from OpenGL.GL import *
from OpenGL.GLU import *

# initial position and size
x1 = GLfloat(0.0).value
y1 = GLfloat(0.0).value
rect_size = GLfloat(25.0).value

# number of pixels to move each step
xstep = GLfloat(1.0).value
ystep = GLfloat(1.0).value

# initialize bouncing window
windowWidth = GLfloat(133).value      # 800/600 = 1.33
windowHeight = GLfloat(100).value
```
```

在 RenderScene() 函数中，我们现在使用双缓冲：

```
def RenderScene():              # display callback function
    glClear(GL_COLOR_BUFFER_BIT)      # clear window with color defined in SetupRC
    #   R   G   B                  # set drawing color to Red
    glColor3f(1.0, 0.0, 0.0)          # functions expects 3 floats
    glRectf(x1, y1, x1 + rect_size, y1 - rect_size) # draw a filled rectangle with above color
    
    glutSwapBuffers()                 # flush and swap double buffers
```

我们必须稍微更改我们的 main() 以将 GLUT_DOUBLE 传递给 glutInitDisplayMode() 函数：

```
def main():
    glutInit()
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB)      # Double buffer
    glutInitWindowSize(800, 600)                      # window size
    glutCreateWindow(b"Bouncing Red Square")          # Python 3: bytes instead of string for Title
    glutDisplayFunc(RenderScene)
    glutReshapeFunc(ChangeSize)
    glutTimerFunc(33, TimerFunction, 1)
    
    SetupRC()
    
    glutMainLoop()

#================
main()
```

我们创建 TimerFunction() 以传递给 glutTimerFunc()。在此函数中，我们进行一些数学计算来定位红色方块并使其在窗口墙壁上反弹。在函数末尾，我们在 glutTimerFunc() 中递归调用此函数：

```
def TimerFunction(value):
    global x1, xstep, y1, ystep

    # reverse direction left/right
    if ((x1 > windowWidth - rect_size) or (x1 < -windowWidth)):
        xstep = -xstep

    # reverse direction top/bottom
    if ((y1 > windowHeight) or (y1 < -windowHeight + rect_size)):
        ystep = -ystep

    # move the red square
    x1 += xstep
    y1 += ystep

    # check the bounds of the clipping area
    if (x1 > (windowWidth - rect_size + xstep)):
        x1 = windowWidth - rect_size - 1
    elif (x1 < -(windowWidth + xstep)):
        x1 = -windowWidth - 1

    if (y1 > (windowHeight + ystep)):
        y1 = windowHeight - 1
    elif (y1 < -(windowHeight - rect_size + ystep)):
        y1 = -windowHeight + rect_size - 1

    # redraw the scene
    glutPostRedisplay()
    glutTimerFunc(33, TimerFunction, 1)    # recursive call
```

当我们现在运行程序时，红色方块将在蓝色窗口内移动，并在每个角落反弹：

```
OpenGL_SuperBible_Animation.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_390_0.png)

## How it works...

在本教程中，我们直接从 Python 使用 OpenGL 库。我们在不使用任何其他 Python GUI 框架的情况下创建了自己的窗口。我们探索了许多 OpenGL 函数中的一些，并创建了可工作的程序。

有关 OpenGL 的简短介绍以及如何从 Python 使用它，可以在 https://noobtuts.com/python/opengl-introduction 上找到。

## Creating a slide show using tkinter

在本教程中，我们将使用纯 Python 创建一个漂亮的可工作幻灯片放映 GUI。我们将看到核心 Python 内置功能的局限性，然后我们将探索另一个可用的第三方模块 Pillow，该模块扩展了 tkinter 在图像处理方面的内置功能。

虽然名字 `Pillow` 最初听起来可能有点奇怪，但它实际上背后有着丰富的历史。

本书中我们仅使用 Python 3.6 及以上版本。我们不会回到 Python 2。Guido 表达了他有意破坏向后兼容性的决定，并决定 Python 3 是 Python 编程的未来。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_391_0.png)

对于 GUI 和图像，旧的 Python 2 行有一个非常强大的模块名为 `PIL`，代表 Python Image Library。该库包含非常大量的功能，在 Python 3 非常成功创建后的几年里，这些功能尚未为 Python 3 翻译。

许多开发人员仍然选择使用 Python 2 而不是由 *Python 的仁慈独裁者* 设计的未来，因为 Python 2 仍然有更多可用的库。

这有点令人难过。幸运的是，已经创建了另一个与 Python 3.6 配合使用的成像库，它的名字是 `PIL` 加上某物。

Pillow 与 Python 2 PIL 库不兼容。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_391_1.png)

## Getting ready

在本教程的第一部分，我们将使用纯 Python。为了改进代码，我们将使用 `pip` 功能安装另一个 Python 模块，因此虽然你可能已经熟悉 `pip`，但了解如何使用它可能会有用。

## How to do it…

首先，我们将创建一个使用纯 Python 在窗口框架内随机播放幻灯片的可工作 GUI。以下是可工作的代码以及运行此代码结果的一些屏幕截图：

```
```python
from tkinter import Tk, PhotoImage, Label
from itertools import cycle
from os import listdir

class SlideShow(Tk):
    # inherit GUI framework extending tkinter
    def __init__(self, mssShowTimeBetweenslides=1500):
```
```

```
# initialize tkinter super class
Tk.__init__(self)

# time each slide will be shown
self.showTime = msShowTimeBetweenSlides

# look for images in current working directory
listOfSlides = [slide for slide in listdir()
                if slide.endswith('gif')]

# cycle slides to show on the tkinter Label
self.iterableCycle = cycle((PhotoImage(file=slide), slide)
                          for slide in listOfSlides)

# create tkinter Label widget which can display images
self.slidesLabel = Label(self)

# create the Frame widget
self.slidesLabel.pack()

def slidesCallback(self):
    # get next slide from iterable cycle
    currentInstance, nameOfSlide = next(self.iterableCycle)

    # assign next slide to Label widget
    self.slidesLabel.config(image=currentInstance)

    # update Window title with current slide
    self.title(nameOfSlide)

    # recursively repeat the Show
    self.after(self.showTime, self.slidesCallback)

#================================
# Start GUI
#================================
win = SlideShow()
win.after(0, win.slidesCallback())
win.mainloop()

SlideShow.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_393_0.png)

以下是幻灯片放映展开过程中的另一个时刻：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_393_1.png)

虽然幻灯片滑动效果确实令人印象深刻，但纯 Python tkinter GUI 的内置功能不支持非常流行的 .jpg 格式，因此我们必须求助于另一个 Python 库。为了使用 Pillow，我们首先必须使用 pip 命令安装它。

一次成功的安装如下所示：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_394_0.png)

Pillow 支持 `.jpg` 格式，为了使用它，我们必须稍微更改语法。

不使用 Pillow，尝试显示 `.jpg` 图像会产生以下错误：

```
SlideShow_try_jpg.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_394_1.png)

使用 Pillow，我们可以同时显示 `.gif` 和 `.jpg` 文件。在成功安装 Pillow 包后，我们只需要进行一些更改：

```
# using Pillow instead of PIL (2.7) for Python 3.6
# Installation is: >pip install Pillow
from PIL import ImageTk

class SlideShow(Tk):
    # inherit GUI framework extending tkinter
    def __init__(self, msShowTimeBetweenSlides=1500):
        # initialize tkinter super class
        Tk.__init__(self)

        # time each slide will be shown
        self.showTime = msShowTimeBetweenSlides

        # look for images in current working directory where this module lives
        # listOfSlides = [slide for slide in listdir() if slide.endswith('gif')]
        listOfSlides = [slide for slide in listdir() if slide.endswith('gif') or slide.endswith('jpg')]

        # endlessly read in the slides so we can show them on the tkinter label
        # self.iterableCycle = cycle((PhotoImage(file=slide), slide) for slide in listOfSlides)
        self.iterableCycle = cycle((ImageTk.PhotoImage(file=slide), slide) for slide in listOfSlides)
```

我们搜索文件夹中的 .jpg 扩展名，并且不再使用 PhotoImage() 类，而是现在使用 ImageTk.PhotoImage() 类。这使我们能够显示具有不同扩展名的图像：

SlideShow_Pillow.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_395_0.png)

