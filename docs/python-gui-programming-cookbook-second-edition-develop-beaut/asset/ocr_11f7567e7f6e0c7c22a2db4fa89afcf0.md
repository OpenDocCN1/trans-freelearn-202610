# Best Practices

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