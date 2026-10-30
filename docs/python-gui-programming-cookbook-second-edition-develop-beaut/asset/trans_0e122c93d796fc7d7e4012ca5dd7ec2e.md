

不过，我们可以使用变量名或函数名加下划线前缀的方式来提示该名称应被视为私有辅助函数。

同时，如果我们希望使用原本是 Python 内置名称的单词，可以在其后加一个下划线后缀。例如，如果我们想用缩写表示列表的长度，可以这样做：

```
len_ = len(aList)
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_147_0.png)

通常，下划线难以阅读且容易被忽略，所以这在实践中可能不是最好的做法。

## 创建可复用的 GUI 组件

我们将使用 Python 创建可复用的 GUI 组件。在本节中，我们将保持简单，把 `ToolTip` 类移动到它自己的模块中。然后，我们将导入并使用它，以便在 GUI 的多个控件上显示工具提示。

## 准备工作

我们的代码基于第 3 章《外观与感觉定制：GUI_tooltip.py》构建。

## 操作步骤……

我们将先把 `ToolTip` 类拆分成一个独立的 Python 模块。我们会对其进行轻微增强，以便传入控件控件以及当鼠标悬停在该控件上时希望显示的工具提示文本。

我们创建一个新的 Python 模块，将 `ToolTip` 类的代码放入其中，然后将此模块导入到我们的主模块中。

然后，我们通过创建多个工具提示来复用导入的 `ToolTip` 类，当鼠标悬停在多个 GUI 控件上时可以看到这些提示。

将我们通用的 `ToolTip` 类代码重构到独立模块中，有助于我们从这个模块复用代码。我们不再复制/粘贴/修改，而是遵循 DRY 原则，将通用代码只放在一个位置，这样当我们修改代码时，所有导入该模块的模块都会自动获得最新版本。

**DRY** 代表 **Don't Repeat Yourself**（不要重复自己），我们将在后面的章节中再次讨论它。我们也可以通过将 **Tab 3** 图像转换为可复用组件来做类似的事情。为了保持本节代码的简洁，我们移除了 **Tab 3**，但你可以用上一章的代码进行实验。该代码从 `GUI_tooltip.py` 中复用。

考虑运行 `GUI_tooltip.py`：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_148_0.png)

考虑以下代码：

```
# Add a Tooltip to the Spinbox
tt.create_ToolTip(self.spin, 'This is a Spinbox control')

# Add Tooltips to more widgets
tt.create_ToolTip(self.name_entered, 'This is an Entry control')
tt.create_ToolTip(self.action, 'This is a Button control')
tt.create_ToolTip(self.scrol, 'This is a ScrolledText control')
```

这在第二个选项卡上同样有效：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_149_0.png)

导入语句如下所示：

```
13  from tkinter import messagebox as msg
14  from tkinter import Spinbox
15  from time import sleep
16  import Ch04_Code.ToolTip as tt
17
18  GLOBAL_CONST = 42
19
20  #==============================================
21  class OOP():
22      def __init__(self):      # Initializer method
23          # Create instance
24          self.win = tk.Tk()
25
26          tt.create_ToolTip(self.win, 'Hello GUI')
```

新的代码结构现在如下所示：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_150_0.png)

而拆分（即重构）后的代码在独立模块中看起来像这样：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_150_1.png)

## Matplotlib 图表

在本章中，我们将使用 Python 3.6 和 `Matplotlib` 模块创建漂亮的图表。我们将涵盖以下内容：

- 使用 `Matplotlib` 创建漂亮的图表
- 使用 pip 配合 whl 扩展名安装 `Matplotlib`
- 创建我们的第一个图表
- 在图表上放置标签
- 如何为图表添加图例
- 缩放图表
- 动态调整图表的缩放比例

### 引言

在本章中，我们将创建美观的图表来直观地表示数据。根据数据源的格式，我们可以在同一图表中绘制一列或多列数据。

我们将使用 Python `Matplotlib` 模块来创建图表。

为了创建这些图形图表，我们需要下载额外的 Python 模块，有几种方法可以安装它们。

本章将说明如何下载 `Matplotlib` Python 模块以及所有其他必需的 Python 模块，以及实现此目的的方法。

在安装所需的模块后，我们将创建自己的 Python 风格图表。

以下是本章涉及 Python 模块的概览：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_154_0.png)

## 使用 Matplotlib 创建漂亮的图表

本节向我们介绍 `Matplotlib` Python 模块，它使我们能够使用 Python 3.6 及以上版本创建可视化图表。

网址 `http://matplotlib.org/users/screenshots.html` 是探索 Matplotlib 世界的一个很好的起点，它还教我们如何创建本章未介绍的许多图表。

### 准备工作

为了使用 `Matplotlib` Python 模块，我们首先必须安装该模块以及其他几个相关的 Python 模块。

如果你运行的是旧于 3.6 的 Python 版本，我建议你升级版本，因为本章将使用 Python `pip` 模块来安装所需的 Python 模块，而 `pip` 随 3.6 及以上版本一起安装。

虽然可以在更早的 Python 3 版本上安装 `pip`，但该过程不太直观，因此绝对最好升级到 3.6 或更高版本。

## 操作步骤……

下图是一个示例，展示了使用 Python 和 `Matplotlib` 模块可以创建的令人惊叹的图形图表：

```
Matplotlib_chart.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_155_0.png)

在下面的代码片段中，我从 http://matplotlib.org/ 网站上复制了一些创建此精美图表的代码。我还添加了一些注释。这个网站上有许多示例，我鼓励你尝试这些示例，直到找到你想创建的图表类型。

以下代码用不到 25 行 Python 代码（包括空格）创建了该图表：

```
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
import matplotlib.pyplot as plt
import numpy as np

fig = plt.figure()                                    # create a figure
ax = fig.gca(projection='3d')                        # create a 3-dimensional axis
X = np.arange(-5, 5, 0.25)                            # horizontal range
Y = np.arange(-5, 5, 0.25)                            # vertical range
X, Y = np.meshgrid(X, Y)                              # create a special grid
R = np.sqrt(X**2 + Y**2)                              # calculate square root
Z = np.sin(R)                                         # calculate sinus

## use #@UndefinedVariable below to ignore the error for cm.coolwarm
surf = ax.plot_surface(X, Y, Z, rstride=1, cstride=1,
                       cmap=cm.coolwarm, linewidth=0,
                       antialiased=False)
ax.set_zlim(-1.01, 1.01)                              # z-axis is third dimension

ax.zaxis.set_major_locator(LinearLocator(10))
ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))

fig.colorbar(surf, shrink=0.5, aspect=5)

plt.show()                                            # display the figure
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_156_0.png)

使用 Python 3.6 或更高版本以及 Eclipse PyDev 插件运行上述代码时，可能会显示一些未解析的导入和变量错误。你可以通过 `#@UnresolvedImport` 和 `#@UndefinedVariable` 在 Eclipse 中直接禁用这些错误。

如果你使用 Eclipse 进行开发，忽略这些错误即可，因为代码会成功运行。

## 工作原理……

为了创建如前所示的漂亮图表，我们需要下载 Matplotlib 以及其他几个 Python 模块。

以下内容将引导我们成功下载并安装所有必需的模块，从而使我们能够创建自己的漂亮图表。

## 使用 pip 配合 whl 扩展名安装 Matplotlib

下载额外 Python 模块的常用方法是使用 `pip`。pip 模块随最新版本的 Python（3.6 及以上）预装。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_157_0.png)

> 如果你使用的是较旧版本的 Python，你可能需要自行下载 pip 和 setuptools。

本节将展示如何使用 pip 成功安装 Matplotlib。我们将使用 `.whl` 扩展名进行安装，因此本节还将向你展示如何安装 wheel 模块。

### 准备工作

首先，让我们检查是否已经安装了 wheel 模块。wheel 模块对于下载和安装具有 `.whl` 扩展名的 Python 包是必需的。

我们可以使用 pip 查看当前已安装的模块。

