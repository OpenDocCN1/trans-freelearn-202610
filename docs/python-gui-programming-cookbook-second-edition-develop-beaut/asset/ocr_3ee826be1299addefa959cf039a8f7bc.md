If you are not familiar with OOP programming, instantiating an object to create an instance of the class, may sound rather boring.

The principle is quite simple and very similar to creating a Python function via a def statement and then, later in the code, actually calling this function.

In a very similar manner, we first create a blueprint of a class and simply assign it to a variable by adding parentheses to the name of the class, as follows:

```
class AClass():
    pass
instance_of_a_class = AClass()
print(instance_of_a_class)
```

The preceding code prints out a memory address and also shows that our variable now has a reference to this class instance.

The cool thing about OOP is that we can create many instances of the same class.

In our tooltip code, we declare a Python class and explicitly make it inherit from object, which is the foundation of all Python classes. We can also leave it out, as we did in the AClass code example, because it is the default for all Python classes.

After all the necessary tooltip creation code that occurs within the ToolTip class, we switch over to non-OOP Python programming by creating a function just below it.

We define the create_ToolTip() function, and it expects one of our GUI widgets to be passed in as an argument, so we can display a tooltip when we hover our mouse over this control.

The create_ToolTip() function actually creates a new instance of our ToolTip class for every widget we call it for.

We can add a tooltip for our Spinbox widget, as follows:

```
# Add a Tooltip
create_ToolTip(spin, 'This is a Spin control')
```