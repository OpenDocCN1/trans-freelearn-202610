In line 119, we are calling the constructor of the imported Menu module class and pass in our main GUI instance, win. We save an instance of the Menu object in the menu_bar variable. In line 120, we configure our GUI to use the just created Menu as the menu for our GUI.

In order to make this work, we also have to add the menu to the menu bar and give it a label.

The menu item was already added to the menu, but we still have to add the menu to the menu bar:

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

Running this code adds a menu bar with a menu that has a menu item:

![](bbox=[0.185, 0.545, 0.75, 0.745])

![](bbox=[0.07, 0.77, 0.15, 0.83])

If this tkinter menubar syntax seems a little bit confusing, do not worry. This is just the syntax of tkinter to create a menubar. It is not very Pythonic.