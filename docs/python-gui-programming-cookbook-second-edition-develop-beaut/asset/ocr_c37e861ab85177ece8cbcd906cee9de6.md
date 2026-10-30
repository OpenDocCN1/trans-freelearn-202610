Note how the feather default icon in the top-left corner of the GUI changed:

```
GUI_icon.py
```

![](bbox=[0.258, 0.186, 0.678, 0.277])

# How it works...

This is another property that ships with tkinter, which ships with Python 3.6 and above. We use the iconbitmap property to change the icon of our main root window form, by passing in a relative path to an icon. This overrides the default icon of tkinter, replacing it with our icon of choice.

# Using a spin box control

In this recipe, we will use a Spinbox widget, and we will also bind the Enter key on the keyboard to one of our widgets.

# Getting ready

We will use our tabbed GUI and add a Spinbox widget above the ScrolledText control. This simply requires us to increment the ScrolledText row value by one and to insert our new Spinbox control in the row above the Entry widget.

# How to do it...

First, we add the Spinbox control. Place the following code above the ScrolledText widget:

```
# Adding a Spinbox widget
spin = Spinbox(mighty, from_=0, to=10)
spin.grid(column=0, row=2)
```