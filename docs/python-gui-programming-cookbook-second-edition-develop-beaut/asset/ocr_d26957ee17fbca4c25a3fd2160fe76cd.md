```
```python
# Adding a Textbox Entry widget - using self
self.name = tk.StringVar()
name_entered = ttk.Entry(mighty, width=12,
                         textvariable=self.name)
name_entered.grid(column=0, row=1, sticky='W')

# Adding a Button - using self
self.action = ttk.Button(mighty, text="Click Me!",
                         command=self.click_me)
self.action.grid(column=2, row=1)
# ...
# ====================
# Start GUI
# ====================
oop = OOP()              # create an instance of the class
# use instance variable to call mainloop via win
oop.win.mainloop()
```
```

We moved the callback methods to the top of the module, inside the new OOP class. We moved all the widget-creation code into one rather long method, which we call in the initializer of the class.

Technically, deep underneath the hood of the low-level code, Python does have a constructor, yet Python frees us from any worries about this. It is taken care of for us.

Instead, in addition to a real constructor, Python provides us with an initializer.

We are strongly encouraged to use this initializer. We can use it to pass in parameters to our class, initializing variables we wish to use inside our class instance.

![](bbox=[0.071, 0.621, 0.15, 0.681])

In Python, several classes can exist within the same Python module.

Unlike Java, which has a very rigid naming convention (without which it does not work), Python is much more flexible.

![](bbox=[0.071, 0.764, 0.15, 0.824])

We can create multiple classes within the same Python module. Unlike Java, we do not depend on a file name that has to match each class name. Python truly rocks!