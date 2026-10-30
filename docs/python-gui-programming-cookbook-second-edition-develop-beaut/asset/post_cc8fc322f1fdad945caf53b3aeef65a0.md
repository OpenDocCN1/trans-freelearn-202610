
# Quickly adding controls using wxPython

In this recipe, we will recreate the GUI we originally created earlier in this book with tkinter, but this time, we will be using the wxPython library. We will see how easy and quick it is to use the wxPython GUI toolkit to create our own Python GUIs.

We will not recreate the entire functionality we created in the previous chapters. For example, we will not internationalize our wxPython GUI, nor connect it to a MySQL database. We will recreate the visual aspects of the GUI and add some functionality.

> Comparing different libraries gives us the choice of which toolkits to use for our own Python GUI development, and we can combine several of those toolkits in our own Python code.

## Getting ready

Ensure you have the wxPython module installed to follow this recipe.

## How to do it...

First, we create our Python OOP class as we did before, using tkinter, but this time we inherit from and extend the `wx.Frame` class. For reasons of clarity, we no longer call our class OOP but, instead, rename it as `MainFrame`.

> In wxPython, the main GUI window is called a Frame.

We also create a callback method that closes the GUI when we click the **Exit** menu item and declare a light-grey `tuple` as the background color for our GUI:

```
import wx
BACKGROUND_COLOR = (240, 240, 240, 255)

class MainFrame(wx.Frame):
    def __init__(self, *args, **kwargs):
        wx.Frame.__init__(self, *args, **kwargs)
        self.createWidgets()
        self.Show()
```

```
def exitGUI(self, event):      # callback
    self.Destroy()

def createWidgets(self):
    self.CreateStatusBar()      # wxPython built-in method
    self.createMenu()
    self.createNotebook()
```

We then add a tabbed control to our GUI by creating an instance of the wxPython Notebook class and assign it as the parent of our own custom class named Widgets.

The notebook class instance variable has wx.Panel as its parent:

```
#--------------------------------------------------
def createNotebook(self):
    panel = wx.Panel(self)
    notebook = wx.Notebook(panel)
    widgets = Widgets(notebook) # Custom class explained below
    notebook.AddPage(widgets, "Widgets")
    notebook.SetBackgroundColour(BACKGROUND_COLOR)
    # layout
    boxSizer = wx.BoxSizer()
    boxSizer.Add(notebook, 1, wx.EXPAND)
    panel.SetSizerAndFit(boxSizer)
```

In wxPython, the tabbed widget is named Notebook, just as in tkinter.

Every Notebook widget needs to have a parent and, in order to lay out widgets in the Notebook in wxPython, we use different kinds of sizers.

wxPython sizers are layout managers, similar to tkinter's grid layout manager.

Next, we add controls to our Notebook page, and we do this by creating a separate class that inherits from wx.Panel:

```
class Widgets(wx.Panel):
    def __init__(self, parent):
        wx.Panel.__init__(self, parent)
        self.createWidgetsFrame()
        self.addWidgets()
        self.layoutWidgets()
```

## Extending Our GUI with the wxPython Library

We modularize our GUI code by breaking it into small methods, following Python OOP programming protocol, which keeps our code manageable and understandable:

```
```python
#-----------------------------------------------
def createWidgetsFrame(self):
    self.panel = wx.Panel(self)
    staticBox = wx.StaticBox(self.panel, -1, "Widgets Frame")
    self.statBoxSizerV = wx.StaticBoxSizer(staticBox, wx.VERTICAL)
#-----------------------------------------------
def layoutWidgets(self):
    boxSizerV = wx.BoxSizer(wx.VERTICAL)
    boxSizerV.Add(self.statBoxSizerV, 1, wx.ALL)
    self.panel.SetSizer(boxSizerV)
    boxSizerV.SetSizeHints(self.panel)
#-----------------------------------------------
def addWidgets(self):
    self.addCheckBoxes()
    self.addRadioButtons()
    self.addStaticBoxWithLabels()
```
```

> When using wxPython `StaticBox` widgets, in order to successfully lay them out, we use a combination of `StaticBoxSizer` and a regular `BoxSizer`. The wxPython `StaticBox` is very similar to the tkinter `LabelFrame` widget.

Embedding a `StaticBox` within another `StaticBox` is straightforward in tkinter, but using wxPython is a little non-intuitive. One way to make it work is shown in the following code snippet:

```
```python
def addStaticBoxWithLabels(self):
    boxSizerH = wx.BoxSizer(wx.HORIZONTAL)
    staticBox = wx.StaticBox(self.panel, -1, "Labels within a Frame")
    staticBoxSizerV = wx.StaticBoxSizer(staticBox, wx.VERTICAL)
    boxSizerV = wx.BoxSizer(wx.VERTICAL)
    staticText1 = wx.StaticText(self.panel, -1, "Choose a number:")
    boxSizerV.Add(staticText1, 0, wx.ALL)
    staticText2 = wx.StaticText(self.panel, -1, "Label 2")
    boxSizerV.Add(staticText2, 0, wx.ALL)
    #-----------------------------------------------
    staticBoxSizerV.Add(boxSizerV, 0, wx.ALL)
    boxSizerH.Add(staticBoxSizerV)
    #-----------------------------------------------
    # boxSizerH.Add(wx.TextCtrl(self.panel))
    # Add local boxSizer to main frame
    self.statBoxSizerV.Add(boxSizerH, 1, wx.ALL)
```
```

First, we create a horizontal BoxSizer. Next, we create a vertical StaticBoxSizer because we want to arrange two labels in a vertical layout in this frame.

In order to arrange another widget to the right of the embedded StaticBox, we have to assign both the embedded StaticBox with its children controls and the next widget to the horizontal BoxSizer and then assign this BoxSizer, which now contains both our embedded StaticBox and our other widgets, to the main StaticBox.

Does this sound confusing?

You just have to experiment with these sizers to get a feel of how to use them. Start with the code for this recipe and comment out some code or modify some x and y coordinates to see the effects.

It is also good to read the official wxPython documentation to learn more.

![](img/986b0dbf3f539f4f3baa82bd3003f04e_343_0.png)

The important thing is knowing where to add to the different sizers in the code in order to achieve the layout we wish.

In order to create the second StaticBox below the first, we create separate StaticBoxSizers and assign them to the same panel:

```
class Widgets(wx.Panel):
    def __init__(self, parent):
        wx.Panel.__init__(self, parent)
        self.panel = wx.Panel(self)
        self.createWidgetsFrame()
        self.createManageFilesFrame()
        self.addWidgets()
        self.addFileWidgets()
        self.layoutWidgets()

    #------------------------------------------------------------
    def createWidgetsFrame(self):
        staticBox = wx.StaticBox(self.panel, -1, "Widgets Frame",
                                size=(285, -1))
        self.statBoxSizerV = wx.StaticBoxSizer(staticBox, wx.VERTICAL)
    #------------------------------------------------------------
    def createManageFilesFrame(self):
        staticBox = wx.StaticBox(self.panel, -1, "Manage Files",
                                size=(285, -1))
        self.statBoxSizerMgrV = wx.StaticBoxSizer(staticBox, wx.VERTICAL)
    #------------------------------------------------------------
    def layoutWidgets(self):
        boxSizerV = wx.BoxSizer( wx.VERTICAL )
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_345_0.png)

## How it works...

We design and lay out our wxPython GUI in several classes.

Once we have done this, in the bottom section of our Python module, we create an instance of the wxPython application. Next, we instantiate our wxPython GUI code.

After that, we call the main GUI event loop, which executes all of our Python code running within this application process. This displays our wxPython GUI.

Whatever code we place between the creation of the app and calling its main event loop, becomes our wxPython GUI. It might take some time to really get used to the wxPython library and its API, but once we understand how to use it, this library is really fun and a powerful tool to build our own Python GUIs. There is also a visual designer tool that can be used with wxPython:
http://www.cae.tntech.edu/help/programming/wxdesigner-getting-started/view

This recipe used OOP to learn how to use the wxPython GUI toolkit.

## Trying to embed a main wxPython app in a main tkinter app

Now that we have created the same GUI using both Python's built-in tkinter library as well as the wxPython wrapper of the wxWidgets library, we really do need to combine the GUIs we created using these technologies.

Both the wxPython and the tkinter libraries have their own advantages. In online forums such as http://stackoverflow.com/, we often see questions, such as which one is better, which GUI toolkit should I use, and so on. This suggests that we have to make an either-or decision. We do not have to make such a decision.

One of the main challenges in doing so is that each GUI toolkit must have its own event loop.

In this recipe, we will try to embed a simple wxPython GUI by calling it from our tkinter GUI.

## Getting ready

We will reuse the tkinter GUI we built in the recipe Combo Box Widgets, in Chapter 1, Creating the GUI Form and Adding Widgets.
