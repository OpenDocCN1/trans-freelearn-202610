# Getting ready

We will add functionality to the Help | About menu item we created in the previous chapter, in the Creating tabbed widgets recipe.

The code is from GUI_tabbed_all_widgets_both_tabs.py. The typical feedback to the user when clicking the Help | About menu in most applications is informational. We start with this information and then vary the design pattern to show warnings and errors.

# How to do it...

Add the following line of code to the top of the module where the import statements live:

```
#===================
# imports
#===================
import tkinter as tk
from tkinter import ttk
from tkinter import scrolledtext
from tkinter import Menu
from tkinter import messagebox as msg
```

Next, create a callback function that will display a message box. We have to locate the code of the callback above the code where we attach the callback to the menu item, because this is still procedural and not OOP code.

Add the following code just above the lines where we create the help menu:

GUI_message_box.py

```
# Display a Message Box
def _msgBox():
    msg.showinfo('Python Message Info Box', 'A Python GUI created using tkinter:\nThe year is 2017.')
# Add another Menu to the Menu Bar and an item
help_menu = Menu(menu_bar, tearoff=0)
help_menu.add_command(label="About", command=_msgBox)  # display messagebox when clicked
menu_bar.add_cascade(label="Help", menu=help_menu)
```