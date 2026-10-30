
Now, our GUI has a new button and, when we click it, we clear the text of the `ScrolledText` widget. Running `GUI_Complexity_start_add_button.py` gives us that new button:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_429_0.png)

In order to add this functionality, we had to add code in two places in the same Python module.

We inserted the new button in the `createWidgets` method (not shown) and then we created a new callback method, which our new button calls when it is clicked. We placed this code just below the callback of our first button.

Our next feature request is to add more functionality. The business logic is encapsulated in another Python module. We invoke this new functionality by adding three more buttons to **Tab 1**. We use a loop to do this:

```
```python
# Adding more Feature Buttons
for idx in range(3):
    b = ttk.Button(self.monty, text="Feature" + str(idx+1))
    b.grid(column=idx, row=4)
```
```

Our GUI now looks like this, post running

```
GUI_Complexity_start_add_three_more_buttons.py:
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_430_0.png)

Next, our customers ask for more features and we use the same approach. Our GUI now looks as follows:

```
GUI_Complexity_start_add_three_more_buttons_add_more.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_430_1.png)

> This is not too bad. When we get new feature requests for another 50 new features, we start to wonder if our approach is still the best approach to use...

One way to manage the ever-increasing complexity our GUI has to handle is by adding tabs. By adding more tabs and placing related features into their own tab, we get control of the complexity and make our GUI more intuitive.

Here is the code, `GUI_Complexity_end_tab3.py`, which creates our new **Tab 3**:

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

Running the preceding code gives the following new Python GUI:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_432_0.png)

## How it works…

In this recipe, we added several new widgets to our GUI in order to add more functionality to our Python GUI. We saw how more and more new feature requests easily got our nice GUI designs into a state where it became less clear how to use the GUI.

> Suddenly, widgets took over the world…

We saw how to handle complexity by modularizing our GUI by breaking large features into smaller pieces and arranging them in functionally related areas using tabs.

While complexity has many aspects, modularizing and refactoring the code is usually a very good approach to handling software code complexity.

# GUI design using multiple notebooks

In this recipe, we will create our GUI using multiple notebooks. Surprisingly, `tkinter` does not ship out of the box with this functionality, but we can easily design such a widget.

Using multiple notebooks will further reduce the complexity discussed in the previous recipe.

## Getting ready

We will create a new Python GUI similar to the one in the previous recipe. This time, however, we will design our GUI with two notebooks. In order to focus on this feature, we will use functions instead of class methods. Reading the previous recipe will be a good introduction to this recipe.

## How to do it…

In order to use multiple notebooks within the same GUI, we start by creating two frames. The first frame will hold the notebooks and their tabs, while the second frame will serve as the display area for the widgets each tab is designed to display. We use the grid layout manager to arrange the two frames, placing one above the other. Then, we create two notebooks and arrange them within the first frame:

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

## Best Practices

Next, we use a loop to create five tabs and add them to each notebook:

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

We create a callback function and bind the click event of the two notebooks to this callback function. Now, when the user clicks on any tab belonging to the two notebooks, this callback function will be called:

```
```python
# bind click-events to Notebooks
note1.bind("<ButtonRelease-1>", notebook_callback)
note2.bind("<ButtonRelease-1>", notebook_callback)
```
```

In the callback function, we add logic that decides which widgets get displayed after clicking a tab:

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

We add a function that creates a display area and another function that clears the area:

Note how the callback function calls the clear_display_area() function.

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

The clear_display_area() function knows both the row and column in which the widgets of tabs are being created, and by finding row zero, we can then use grid_forget() to clear the display.

For Tabs 1 to 3 of the first notebook, we create new frames to hold more widgets. Clicking any of those three tabs then results in a GUI very similar to the one we created in the previous recipe.

These first three tabs are being invoked in the callback function as display_tab1(), display_tab2(), and display_tab3() when those tabs are being clicked.
