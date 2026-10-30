# # Mixing fall-down and OOP coding

Python is an object-oriented programming language, yet it does not always make sense to use OOP. For simple scripting tasks, the legacy waterfall coding style is still appropriate.

In this recipe, we will create a new GUI that mixes both the fall-down coding style with the more modern OOP coding style.

We will create an OOP-style class that will display a tooltip when we hover the mouse over a widget in a Python GUI that we will create using the waterfall style.

![](bbox=[0.117, 0.318, 0.196, 0.375])

> Fall-down and waterfall coding styles are the same. It means that we have to physically place code above code before we can call it from the code below. In this paradigm, the code literally falls down from the top of our program to the bottom of our program when we execute the code.

# ## Getting ready

In this recipe, we will create a GUI using `tkinter`, which is similar to the GUI we created in the first chapter of this book.

# ## How to do it...

In Python, we can bind functions to classes by turning them into methods using the `self` naming convention. This is a truly wonderful capability of Python, and it allows us to create large systems that are understandable and maintainable.

Sometimes, when we only write short scripts, OOP does not make sense because we find ourselves prepending a lot of variables with the `self` naming convention and the code gets unnecessarily large when it does not need to be.

Let's first create a Python GUI using `tkinter` and code it in the waterfall style.

The following code `GUI_FallDown.py` creates the GUI:

```
```python
#========================
# imports
#========================
import tkinter as tk
from tkinter import ttk
from tkinter import messagebox
```
```