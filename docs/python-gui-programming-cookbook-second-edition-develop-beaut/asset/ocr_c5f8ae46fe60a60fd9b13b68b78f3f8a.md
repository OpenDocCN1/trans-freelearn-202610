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

Now, we can see a separator line in between our two menu items:

![](bbox=[0.18, 0.35, 0.76, 0.56])

By passing in the tearoff property to the constructor of the menu, we can remove the first dashed line that, by default, appears above the first menu item in a menu:

```
# Add menu items
file_menu = Menu(menu_bar, tearoff=0)
file_menu.add_command(label="New")
file_menu.add_separator()
```