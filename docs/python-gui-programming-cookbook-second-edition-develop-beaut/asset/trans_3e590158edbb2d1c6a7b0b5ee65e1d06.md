

# 如何实现...

这里，我们将创建一个顶级框架，用来容纳其他框架和小部件。这将帮助我们获得想要的GUI布局。

为了实现这一点，我们必须将当前控件嵌入到一个名为 `ttk.LabelFrame` 的中央框架中。该 `ttk.LabelFrame` 框架是主父窗口的子级，所有控件都将成为这个 `ttk.LabelFrame` 的子级。

到目前为止，在我们的教程中，我们一直将所有小部件直接分配给主GUI框架。现在，我们只将 `LabelFrame` 分配给主窗口，之后让这个 `LabelFrame` 成为所有小部件的父容器。

这会在我们的GUI布局中创建如下层次结构：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_73_0.png)

在上图中，**win** 是保存对主GUI tkinter窗口框架引用的变量，**mighty** 是保存对 `LabelFrame` 引用的变量，并且是主窗口框架 (**win**) 的子级，而 **Label** 及所有其他小部件现在都被放入 `LabelFrame` 容器 (**mighty**) 中。

将以下代码添加到Python模块的顶部：

GUI_embed_frames.py

```
6 #====================
7 # imports
8 #====================
9 import tkinter as tk
10 from tkinter import ttk
11 from tkinter import scrolledtext
12 
13 # Create instance
14 win = tk.Tk()
15 
16 # Add a title
17 win.title("Python GUI")
18 
19 # We are creating a container frame to hold all other widgets
20 mighty = ttk.LabelFrame(win, text=' Mighty Python ')
21 mighty.grid(column=0, row=0, padx=8, pady=4)
```

接下来，我们将修改以下控件，使用 mighty 作为父级，替换 win。下面是一个如何操作的示例：

```
23 # Modify adding a Label using mighty as the parent instead of win
24 a_label = ttk.Label(mighty, text="Enter a name:")
25 a_label.grid(column=0, row=0)
```

这会产生如下GUI：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_74_0.png)

让我们将第0列中的输入框左对齐：

```
# Adding a Textbox Entry widget
name = tk.StringVar()
name_entered = ttk.Entry(mighty, width=12, textvariable=name)
name_entered.grid(column=0, row=1, sticky=tk.W)      # align left/West
```

现在，标签和输入框都向 West（左）对齐：

GUI_embed_frames_align_entry_west.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_77_0.png)

为了将 Frame LabelFrame 中标签长度对我们GUI布局其余部分的影响隔离开来，我们不能将这个 LabelFrame 与其他小部件放在同一个 LabelFrame 中，而必须将其直接分配给主GUI窗体 (win)。我们将在后续章节中这样做。

## 创建菜单栏

在本节中，我们将为主窗口添加菜单栏，在菜单栏中添加菜单，然后向菜单添加菜单项。

## 准备工作

我们将首先学习如何添加菜单栏、多个菜单以及几个菜单项的技术，以展示其操作原理。最初，单击菜单项不会产生任何效果。然后，我们将为菜单项添加功能，例如单击 Exit 菜单项时关闭主窗口，以及显示 Help | About 对话框。

我们将继续扩展上一个教程中创建的GUI，即通过“在框架中嵌入框架”来对齐GUI小部件。

## 如何实现...

首先，我们必须从 tkinter 导入 Menu 类。将以下代码行添加到Python模块的顶部，即import语句所在的位置：

```
6 # ====================
7 # imports
8 # ====================
9 import tkinter as tk
10 from tkinter import ttk
11 from tkinter import scrolledtext
12 from tkinter import Menu
13 
14 # Create instance
15 win = tk.Tk()
```

接下来，我们将创建菜单栏。将以下代码添加到模块底部，即创建主事件循环的代码上方：

GUI_menubar_tearoff.py

```
118 # Creating a Menu Bar
119 menu_bar = Menu(win)
120 win.config(menu=menu_bar)
121 
122 # Create menu and add menu items
123 file_menu = Menu(menu_bar)                # create File menu
124 file_menu.add_command(label="New")       # add File menu item
```

在第119行，我们调用导入的 Menu 模块类的构造函数，并传入主GUI实例 win。我们将 Menu 对象的一个实例保存在 menu_bar 变量中。在第120行，我们将GUI配置为使用刚刚创建的 Menu 作为GUI的菜单。

为了使其正常工作，我们还需要将菜单添加到菜单栏，并为其指定一个标签。

菜单项已经添加到菜单中，但我们仍然必须将菜单添加到菜单栏：

GUI_menubar_file.py

```
122 # Create menu and add menu items
123 file_menu = Menu(menu_bar)                  # create File menu
124 file_menu.add_command(label="New")          # add File menu item
125 menu_bar.add_cascade(label="File", menu=file_menu)  # add File menu to menu bar and give it a label
126
127 name_entered.focus()        # Place cursor into name Entry
128 #========================
129 # Start GUI
130 #========================
131 win.mainloop()
```

运行此代码会添加一个带有一个菜单项的菜单栏：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_79_0.png)

![](img/986b0dbf3f539f4f3baa82bd3003f04e_79_1.png)

如果这个 tkinter 菜单栏语法看起来有点令人困惑，不必担心。这只是 tkinter 创建菜单栏的语法。它并不十分 Pythonic。

## 布局管理

接下来，我们将在添加到菜单栏的第一个菜单中添加第二个菜单项：

```
GUI_menubar_exit.py
```

```
```python
# Add menu items
file_menu = Menu(menu_bar)
file_menu.add_command(label="New")
file_menu.add_command(label="Exit")
menu_bar.add_cascade(label="File", menu=file_menu)

name_entered.focus()        # Place cursor into name Entry
#====================
# Start GUI
#====================
win.mainloop()
```
```

运行代码会产生以下结果：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_80_0.png)

我们可以在现有菜单项之间添加一行代码，以添加分隔线：

```
GUI_menubar_separator.py
```

```
# Creating a Menu Bar
menu_bar = Menu(win)
win.config(menu=menu_bar)

# Add menu items
file_menu = Menu(menu_bar)
file_menu.add_command(label="New")
file_menu.add_separator()
file_menu.add_command(label="Exit")
menu_bar.add_cascade(label="File", menu=file_menu)
```

现在，我们可以在两个菜单项之间看到一条分隔线：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_81_0.png)

通过将 tearoff 属性传递给菜单的构造函数，我们可以移除默认出现在菜单第一个菜单项上方的第一条虚线：

```
# Add menu items
file_menu = Menu(menu_bar, tearoff=0)
file_menu.add_command(label="New")
file_menu.add_separator()
```

现在，虚线不再出现，我们的GUI看起来好多了：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_82_0.png)

接下来，我们将添加第二个菜单，它将被水平放置到第一个菜单的右侧。我们给它一个菜单项，命名为 About，并且为了使其正常工作，我们必须将第二个菜单添加到菜单栏。

File 和 Help | About 是我们非常熟悉的常见 Windows GUI 布局，我们可以使用 Python 和 tkinter 创建相同的菜单：

GUI_menubar_help.py

```
# Creating a Menu Bar
menu_bar = Menu(win)
win.config(menu=menu_bar)

# Add menu items
file_menu = Menu(menu_bar, tearoff=0)
file_menu.add_command(label="New")
file_menu.add_separator()
file_menu.add_command(label="Exit")
menu_bar.add_cascade(label="File", menu=file_menu)

# Add another Menu to the Menu Bar and an item
help_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="Help", menu=help_menu)
help_menu.add_command(label="About")

name_entered.focus()      # Place cursor into name Entry
====================
# Start GUI
====================
win.mainloop()
```

