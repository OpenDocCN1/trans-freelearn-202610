Next, we add another property to customize our widget further; bd is a short-hand notation for the borderwidth property:

```
spin = Spinbox(mighty, from_=0, to=10, width=5 , bd=8)
```

Running the preceding code results in the following GUI:

GUI_spinbox_small_bd.py

![](bbox=[0.22, 0.26, 0.71, 0.55])

Here, we add functionality to the widget by creating a callback and linking it to the control.

This will print the selection of the Spinbox into ScrolledText as well as onto stdout.
The variable named scrol is our reference to the ScrolledText widget:

```
# Spinbox callback
def _spin():
    value = spin.get()
    print(value)
    scrol.insert(tk.INSERT, value + 'n')

spin = Spinbox(mighty, from_=0, to=10, width=5, bd=8,
                command=_spin)
```