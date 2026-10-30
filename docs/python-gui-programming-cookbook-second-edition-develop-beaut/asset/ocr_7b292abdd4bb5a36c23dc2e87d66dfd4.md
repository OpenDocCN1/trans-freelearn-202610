# # Using module-level global variables

Encapsulation is a major strength in any programming language, enabling us to program using OOP. Python is both OOP as well as procedural. We can create global variables that are localized to the module they reside in. They are global only to this module, which is one form of encapsulation. Why do we want this? Because as we add more and more functionality to our GUI, we want to avoid naming conflicts that could result in bugs in our code.

> > We do not want naming clashes creating bugs in our code! Namespaces are one way to avoid these bugs, and in Python, we can do this by using Python modules (which are unofficial namespaces).

# ## Getting ready

We can declare module-level **globals** in any module just above and outside functions.

We then have to use the `global` Python keyword to refer to them. If we forget to use `global` in functions, we will accidentally create new local variables. This would be a bug and something we really do not want to do.

> > Python is a dynamic, strongly typed language. We will notice bugs such as this (forgetting to scope variables with the `global` keyword) only at runtime.

# ## How to do it…

Add the code shown on line 17 to the GUI we used in the previous recipe, *How to get data from a widget*, creating a module-level global variable. We use the C-style all uppercase convention, which is not truly Pythonic, but I think this does emphasize the principle we are addressing in this recipe: