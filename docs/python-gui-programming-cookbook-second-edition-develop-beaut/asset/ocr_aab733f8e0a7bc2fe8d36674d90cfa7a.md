```
class ButtonRidge(ButtonBase):
    relief  ='ridge'
    foreground ='red'

class ButtonSunken(ButtonBase):
    relief  ='sunken'
    foreground ='blue'

class ButtonGroove(ButtonBase):
    relief  ='groove'
    foreground ='green'

buttonTypes = [ButtonRidge, ButtonSunken, ButtonGroove]

class OOP():
    def __init__(self):
        self.win = tk.Tk()
        self.win.title("Python GUI")
        self.createWidgets()
```

We create a base class that our different button style classes inherit from and in which each of them overrides the relief and foreground configuration properties. All subclasses inherit the getButtonConfig method from this base class. This method returns a tuple.

We also create a button factory class and a list that holds the names of our button subclasses. We name the list buttonTypes, as our factory will create different types of buttons.

Further down in the module, we create the button widgets, using the same buttonTypes list:

```
def createButtons(self):

    factory = ButtonFactory()

    # Button 1
    rel = factory.createButton(0).getButtonConfig()[0]
    fg  = factory.createButton(0).getButtonConfig()[1]
    action = tk.Button(self.monty, text="Button "+str(0+1),
                       relief=rel, foreground=fg)
    action.grid(column=0, row=1)

    # Button 2
    rel = factory.createButton(1).getButtonConfig()[0]
    fg  = factory.createButton(1).getButtonConfig()[1]
    action = tk.Button(self.monty, text="Button "+str(1+1),
                       relief=rel, foreground=fg)
```