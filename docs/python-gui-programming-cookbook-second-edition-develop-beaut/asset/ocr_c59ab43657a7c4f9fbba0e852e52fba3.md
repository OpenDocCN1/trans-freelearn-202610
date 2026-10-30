# Getting ready

We will start by learning the techniques of how to add a menu bar, several menus, and a few menu items to show the principle of how to do it. In the beginning, clicking on a menu item will have no effect. We will then add functionality to the menu items, for example, closing the main window when clicking the Exit menu item and displaying a Help | About dialog.

We will continue to extend the GUI we created in the previous recipe, Aligning the GUI widgets by embedding frames within frames.

# How to do it...

First, we'll have to import the Menu class from tkinter. Add the following line of code to the top of the Python module, where the import statements live:

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

Next, we will create the menu bar. Add the following code towards the bottom of the module, just above where we create the main event loop:

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