

现在，我们的 GUI 有了一个新按钮，点击它时，我们会清除 `ScrolledText` 小部件的文本。运行 `GUI_Complexity_start_add_button.py` 会得到这个新按钮：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_429_0.png)

为了添加此功能，我们必须在同一个 Python 模块的两个地方添加代码。

我们在 `createWidgets` 方法中插入了新按钮（未显示），然后创建了一个新的回调方法，我们的新按钮在被点击时会调用它。我们将此代码放在第一个按钮的回调下方。

我们的下一个功能请求是添加更多功能。业务逻辑封装在另一个 Python 模块中。我们通过向 **Tab 1** 添加三个更多按钮来调用此新功能。我们使用循环来执行此操作：

```
```python
# Adding more Feature Buttons
for idx in range(3):
    b = ttk.Button(self.monty, text="Feature" + str(idx+1))
    b.grid(column=idx, row=4)
```
```

运行后，我们的 GUI 现在看起来像这样

```
GUI_Complexity_start_add_three_more_buttons.py:
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_430_0.png)

接下来，我们的客户要求更多功能，我们使用相同的方法。我们的 GUI 现在如下所示：

```
GUI_Complexity_start_add_three_more_buttons_add_more.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_430_1.png)

> 这还不算太糟糕。当我们为另外 50 个新功能收到新功能请求时，我们开始怀疑我们的方法是否仍然是最好的方法...

管理 GUI 必须处理的不断增加的复杂性的一种方法是添加选项卡。通过添加更多选项卡并将相关功能放入各自的选项卡中，我们可以控制复杂性并使我们的 GUI 更直观。

以下是代码 `GUI_Complexity_end_tab3.py`，它创建了我们的新 **Tab 3**：

```
```python
# Tab Control 3  ---------------------------------------
        tab3 = ttk.Frame(tabControl)          # Add a tab
        tabControl.add(tab3, text='Tab 3')     # Make tab visible

        monty3 = ttk.LabelFrame(tab3, text=' New Features ')
        monty3.grid(column=0, row=0, padx=8, pady=4)

        # Adding more Feature Buttons
        startRow = 4
        for idx in range(24):
            if idx < 2:
                colIdx = idx
                col = colIdx
            else:
                col += 1
            if not idx % 3:
                startRow += 1
                col = 0

            b = ttk.Button(monty3, text="Feature " + str(idx+1))
            b.grid(column=col, row=startRow)

        # Add some space around each label
        for child in monty3.winfo_children():
            child.grid_configure(padx=8)
```
```

运行上述代码会得到以下新的 Python GUI：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_432_0.png)

## 如何工作…

在本教程中，我们向 GUI 添加了多个新小部件，以便为我们的 Python GUI 添加更多功能。我们看到，随着越来越多的新功能请求，我们漂亮的 GUI 设计很容易变得不清晰，难以使用 GUI。

> 突然间，小部件占领了世界…

我们看到了通过模块化 GUI 来处理复杂性的方法，即将大型功能分解为更小的部分，并使用选项卡将它们按功能相关区域排列。

虽然复杂性有很多方面，但模块化和重构代码通常是处理软件代码复杂性的非常好的方法。

# 使用多个笔记本进行 GUI 设计

在本教程中，我们将使用多个笔记本创建 GUI。令人惊讶的是，`tkinter` 并未开箱即用地提供此功能，但我们可以轻松设计此类小部件。

使用多个笔记本将进一步降低上一教程中讨论的复杂性。

## 准备工作

我们将创建一个新的 Python GUI，类似于上一教程中的 GUI。这次，我们将使用两个笔记本设计我们的 GUI。为了专注于此功能，我们将使用函数而不是类方法。阅读上一教程将是对本教程的良好介绍。

## 如何操作…

为了在同一个 GUI 中使用多个笔记本，我们首先创建两个框架。第一个框架将容纳笔记本及其选项卡，而第二个框架将作为每个选项卡设计的显示小部件的显示区域。我们使用网格布局管理器来排列这两个框架，将一个放在另一个之上。然后，我们创建两个笔记本并将它们排列在第一个框架内：

```
```python
# --------------------------------------------------
# Create GUI
# --------------------------------------------------
win = tk.Tk()                 # Create instance
win.title("Python GUI")       # Add title
# --------------------------------------------------

win_frame_multi_row_tabs = ttk.Frame(win)
win_frame_multi_row_tabs.grid(column=0, row=0, sticky='W')

display_area = ttk.Labelframe(win, text=' Tab Display Area ')
display_area.grid(column=0, row=1, sticky='NW')

note1 = ttk.Notebook(win_frame_multi_row_tabs)
note1.grid(column=0, row=0)

note2 = ttk.Notebook(win_frame_multi_row_tabs)
note2.grid(column=0, row=1)
```
```

## 最佳实践

接下来，我们使用循环创建五个选项卡并将它们添加到每个笔记本中：

```
```python
# create and add tabs to Notebooks
for tab_no in range(5):
    tab1 = ttk.Frame(note1, width=0, height=0)             # Create a tab for notebook 1
    tab2 = ttk.Frame(note2, width=0, height=0)             # Create a tab for notebook 2
    note1.add(tab1, text='Tab {}'.format(tab_no + 1))      # Add tab notebook 1
    note2.add(tab2, text='Tab {}'.format(tab_no + 1))      # Add tab notebook 2
```
```

我们创建一个回调函数并将两个笔记本的点击事件绑定到此回调函数。现在，当用户点击两个笔记本中的任何选项卡时，此回调函数将被调用：

```
```python
# bind click-events to Notebooks
note1.bind("<ButtonRelease-1>", notebook_callback)
note2.bind("<ButtonRelease-1>", notebook_callback)
```
```

在回调函数中，我们添加逻辑来决定点击选项卡后显示哪些小部件：

```
```python
#--------------------------------------------------
def notebook_callback(event):
    clear_display_area()

    current_notebook = str(event.widget)
    tab_no = str(event.widget.index("current") + 1)

    if current_notebook.endswith('notebook'):
        active_notebook = 'Notebook 1'
    elif current_notebook.endswith('notebook2'):
        active_notebook = 'Notebook 2'
    else:
        active_notebook = ''

    if active_notebook is 'Notebook 1':
        if tab_no == '1': display_tab1()
        elif tab_no == '2': display_tab2()
        elif tab_no == '3': display_tab3()
        else: display_button(active_notebook, tab_no)
    else:
        display_button(active_notebook, tab_no)
```
```

我们添加一个创建显示区域的函数和另一个清除该区域的函数：

注意回调函数如何调用 clear_display_area() 函数。

```
#----------------------------------------
def create_display_area():
    # add empty label for spacing
    display_area_label = tk.Label(display_area, text="", height=2)
    display_area_label.grid(column=0, row=0)

#----------------------------------------
def clear_display_area():
    # remove previous widget(s) from display_area:
    for widget in display_area.grid_slaves():
        if int(widget.grid_info()["row"]) == 0:
            widget.grid_forget()
```

clear_display_area() 函数知道选项卡的小部件正在创建的行和列，并且通过找到第零行，我们就可以使用 grid_forget() 来清除显示。

对于第一个笔记本的选项卡 1 到 3，我们创建新框架来容纳更多小部件。点击这三个选项卡中的任何一个都会导致一个与我们在上一教程中创建的非常相似的 GUI。

这三个选项卡在回调函数中作为 display_tab1()、display_tab2() 和 display_tab3() 被调用，当这些选项卡被点击时。

