# Getting ready

Reading one of the previous recipes might be good preparation for this recipe.

In this recipe, we will use slightly modified GUI code with respect to the previous recipe, but most of the basic GUI-building code is the same.

# How to do it...

In the previous recipes, one of our main challenges was how to combine two GUI technologies that were designed to be the one-and-only GUI toolkit for an application. We found various simple ways to combine them.

We will again launch the wxPython GUI from a tkinter GUI main event loop and start the wxPython GUI in its own thread, which runs within the Python.exe process.

In order to do this, we will use a shared global multiprocessing Python Queue.

While it is often best to avoid global data, in this recipe it is a practical solution and Python globals are really only global in the module they have been declared.

Here is the Python code that makes the two GUIs communicate with each other to a certain degree. In order to save space, this is not pure OOP code. Neither are we showing the creation code for all widgets. That code is the same as in the previous recipes:

```
# Communicate.py
import tkinter as tk
from tkinter import ttk
from threading import Thread

win = tk.Tk()
win.title("Python GUI")

from queue import Queue
sharedQueue = Queue()
dataInQueue = False

def putDataIntoQueue(data):
    global dataInQueue
    dataInQueue = True
    sharedQueue.put(data)

def readDataFromQueue():
```