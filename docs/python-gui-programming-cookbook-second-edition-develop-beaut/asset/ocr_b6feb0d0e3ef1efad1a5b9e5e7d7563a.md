Here is the code that runs when clicking on Tab 3 of the first notebook:

```
#----------------------------------------
def display_tab3():
    monty3 = ttk.LabelFrame(display_area, text=' New Features ')
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

        b = ttk.Button(monty3, text="Feature " + str(idx + 1))
        b.grid(column=col, row=startRow)

    # Add some space around each label
    for child in monty3.winfo_children():
        child.grid_configure(padx=8)
```

Clicking any tab other than the first three tabs of notebook one calls the same function, display_button(), which results in a button being displayed whose text property is being set to show the notebook and tab number:

```
#----------------------------------------
def display_button(active_notebook, tab_no):
    btn = ttk.Button(display_area, text=active_notebook + ' - Tab ' + tab_no, \
                     command= lambda: showinfo("Tab Display", "Tab: " + tab_no ))
    btn.grid(column=0, row=0, padx=8, pady=8)
```

Clicking any of these buttons results in a Message box.

At the end of the code, we invoke the display_tab1() function. When the GUI first starts up, the widgets of this tab are what get displayed in the display area: