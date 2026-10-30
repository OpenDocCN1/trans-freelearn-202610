In Python GUI programming, one of the first things that can go wrong is missing out on importing the required modules or importing the existing modules.

Here is a simple example:

GUI.py with the import statement `# import tkinter as tk` commented out:

![](bbox=[0.244, 0.231, 0.701, 0.506])

We are trying to create an instance of the `tkinter` class, but things don't work as expected.

Well, we simply forgot to import the module and alias it as `tk`, and we can fix this by adding a line of Python code above our class creation, where the import statements live:

```
```python
#====================
# imports
#====================
import tkinter as tk
```
```

This is an example in which our development environment does the testing for us. We just have to do the debugging and code fixing.

Another example more closely related to developer testing is when we code conditionals and, during our regular development, do not exercise all branches of logic.

Using an example from Chapter 7, *Storing Data in our MySQL Database via our GUI*, let's say we click on the **Get Quotes** button and this works, but we never clicked on the **Mody Quote** button. The first button click creates the desired result but the second throws an exception (because we had not yet implemented this code and probably forgot all about it):