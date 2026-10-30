Now, the dashed line no longer appears, and our GUI looks so much better:

![](bbox=[0.247, 0.128, 0.815, 0.304])

Next, we'll add a second menu, which will be horizontally placed to the right of the first menu. We'll give it one menu item, which we name About and, in order for this to work, we have to add this second menu to the menu bar.

File and Help | About are very common Windows GUI layouts we are all familiar with, and we can create those same menus using Python and tkinter:

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