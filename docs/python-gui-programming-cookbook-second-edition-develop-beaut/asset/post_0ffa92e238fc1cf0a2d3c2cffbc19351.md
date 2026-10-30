
## Creating the GUI Form and Adding Widgets

In this chapter, we start creating amazing GUIs using Python 3.6 and above. We will cover the following topics:

- Creating our first Python GUI
- Preventing the GUI from being resized
- Adding a label to the GUI form
- Creating buttons and changing their text property
- Text box widgets
- Setting the focus to a widget and disabling widgets
- Combo box widgets
- Creating a check button with different initial states
- Using radio button widgets
- Using scrolled text widgets
- Adding several widgets in a loop

### Introduction

In this chapter, we will develop our first GUI in Python. We will start with the minimum code required to build a running GUI application. Each recipe then adds different widgets to the GUI form.

In the first two recipes, we will show the entire code, consisting of only a few lines of code. In the following recipes, we will only show the code to be added to the previous recipes.

By the end of this chapter, we will have created a working GUI application that consists of labels, buttons, text boxes, combo boxes, check buttons in various states, as well as radio buttons that change the background color of the GUI.

At the beginning of each chapter, I will show the Python modules that belong to each chapter. I will then reference the different modules that belong to the code shown, studied and run.

Here is the overview of Python modules (ending in a .py extension) for this chapter:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_30_0.png)

## Creating our first Python GUI

Python is a very powerful programming language. It ships with the built-in `tkinter` module. In only a few lines of code (four, to be precise) we can build our first Python GUI.

### Getting ready

To follow this recipe, a working Python development environment is a prerequisite. The IDLE GUI, which ships with Python, is enough to start. IDLE was built using tkinter!

- All the recipes in this book were developed using Python 3.6 on a Windows 10 64-bit OS. They have not been tested on any other configuration. As Python is a cross-platform language, the code from each recipe is expected to run everywhere.
- If you are using a Mac, it does come with built-in Python, yet it might be missing some modules such as tkinter, which we will use throughout this book.
- We are using Python 3.6, and the creator of Python intentionally chose not to make it backwards compatible with Python 2. If you are using a Mac or Python 2, you might have to install Python 3.6 from www.python.org in order to successfully run the recipes in this book.
- If you really wish to run the code in this book on Python 2.7, you will have to make some adjustments. For example, tkinter in Python 2.x has an uppercase T. The Python 2.7 print statement is a function in Python 3.6 and requires parentheses.
- While the EOL (End Of Life) for the Python 2.x branch has been extended to the year 2020, I would strongly recommend that you start using Python 3.6 and above.
- Why hold on to the past, unless you really have to? Here is a link to the Python Enhancement Proposal (PEP) 373 that refers to the EOL of Python 2: https://www.python.org/dev/peps/pep-0373/

![](img/986b0dbf3f539f4f3baa82bd3003f04e_31_0.png)

### How to do it…

Here are the four lines of `First_GUI.py` required to create the resulting GUI:

```
```python
6  #====================
7  # imports
8  #====================
9  import tkinter as tk

11 # Create instance
12 win = tk.Tk()

14 # Add a title
15 win.title("Python GUI")

17 #====================
18 # Start GUI
19 #====================
20 win.mainloop()
```
```

Execute this code and admire the result:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_32_0.png)

### How it works…

In line nine, we import the built-in `tkinter` module and alias it as `tk` to simplify our Python code. In line 12, we create an instance of the Tk class by calling its constructor (the parentheses appended to Tk turns the class into an instance). We are using the alias `tk`, so we don’t have to use the longer word `tkinter`. We are assigning the class instance to a variable named `win` (short for a window). As Python is a dynamically typed language, we did not have to declare this variable before assigning to it, and we did not have to give it a specific type. Python infers the type from the assignment of this statement. Python is a strongly typed language, so every variable always has a type. We just don’t have to specify its type beforehand like in other languages. This makes Python a very powerful and productive language to program in.

### A little note about classes and types:

- In Python, every variable always has a type. We cannot create a variable that does not have a type. Yet, in Python, we do not have to declare the type beforehand, as we have to do in the C programming language.
- Python is smart enough to infer the type. C#, at the time of writing this book, also has this capability. Using Python, we can create our own classes using the `class` keyword instead of the `def` keyword.
- In order to assign the class to a variable, we first have to create an instance of our class. We create the instance and assign this instance to our variable, for example:

```
class AClass(object):
    print('Hello from AClass')
class_instance = AClass()
```

Now, the variable, `class_instance`, is of the AClass type. If this sounds confusing, do not worry. We will cover OOP in the coming chapters.

In line 15, we use the instance variable (`win`) of the class to give our window a title via the `title` property. In line 20, we start the window's event loop by calling the `mainloop` method on the class instance, `win`. Up to this point in our code, we created an instance and set one property, but the GUI will not be displayed until we start the main event loop.

- An event loop is a mechanism that makes our GUI work. We can think of it as an endless loop where our GUI is waiting for events to be sent to it. A button click creates an event within our GUI, or our GUI being resized also creates an event.
- We can write all of our GUI code in advance and nothing will be displayed on the user's screen until we call this endless loop (`win.mainloop()`) in the preceding code. The event loop ends when the user clicks the red X button or a widget that we have programmed to end our GUI. When the event loop ends, our GUI also ends.

### There's more...

This recipe used a minimum amount of Python code to create our first GUI program. However, throughout this book we will use OOP when it makes sense.

## Preventing the GUI from being resized

By default, a GUI created using tkinter can be resized. This is not always ideal. The widgets we place onto our GUI forms might end up being resized in an improper way, so in this recipe, we will learn how to prevent our GUI from being resized by the user of our GUI application.

### Getting ready

This recipe extends the previous one, *Creating our first Python GUI*, so one requirement is to have typed the first recipe yourself into a project of your own, or download the code from https://github.com/PacktPublishing/Python-GUI-Programming-Cookbook-Second-Edition/.

### How to do it...

We are preventing the GUI from being resized, look at:

```
GUI_not_resizable.py
```

```
6 #====================
7 # imports
8 #====================
9 import tkinter as tk
10
11 # Create instance
12 win = tk.Tk()
13
14 # Add a title
15 win.title("Python GUI")
16
17 # Disable resizing the GUI by passing in False/False
18 win.resizable(False, False)
19
20 # Enable resizing x-dimension, disable y-dimension
21 # win.resizable(True, False)
22
23 #====================
24 # Start GUI
25 #====================
26 win.mainloop()
```

Running the code creates this GUI:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_35_0.png)

### How it works...

Line 18 prevents the Python GUI from being resized.

Running this code will result in a GUI similar to the one we created in the first recipe. However, the user can no longer resize it. Also, note how the maximize button in the toolbar of the window is grayed out.

Why is this important? Because once we add widgets to our form, resizing can make our GUI look not as good as we want it to be. We will add widgets to our GUI in the next recipes.

The resizable() method is of the Tk() class, and by passing in (False, False), we prevent the GUI from being resized. We can disable both the x and y dimensions of the GUI from being resized, or we can enable one or both dimensions by passing in True or any number other than zero. (True, False) would enable the x-dimension but prevent the y-dimension from being resized.

We also added comments to our code in preparation for the recipes contained in this book.

![](img/986b0dbf3f539f4f3baa82bd3003f04e_36_0.png)

In visual programming IDEs such as Visual Studio .NET, C# programmers often do not think of preventing the user from resizing the GUI they developed in this language. This creates inferior GUIs. Adding this one line of Python code can make our users appreciate our GUI.

## Adding a label to the GUI form

A label is a very simple widget that adds value to our GUI. It explains the purpose of the other widgets, providing additional information. This can guide the user to the meaning of an Entry widget, and it can also explain the data displayed by widgets without the user having to enter data into it.

### Getting ready

We are extending the first recipe, Creating our first Python GUI. We will leave the GUI resizable, so don’t use the code from the second recipe (or comment the win.resizable line out).
