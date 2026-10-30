```
def click_me():
    action.configure(text='Hello ' + name.get() + ' ' +
                    number_chosen.get())

# Adding a Textbox Entry widget
name = tk.StringVar()
name_entered = ttk.Entry(mighty, width=12, textvariable=name)
name_entered.grid(column=0, row=1, sticky='W')

# Adding a Button
action = ttk.Button(mighty, text="Click Me!", command=click_me)
action.grid(column=2, row=1)

ttk.Label(mighty, text="Choose a number:").grid(column=1, row=0)
number = tk.StringVar()
number_chosen = ttk.Combobox(mighty, width=12,
                            textvariable=number, state='readonly')
number_chosen['values'] = (1, 2, 4, 42, 100)
number_chosen.grid(column=1, row=1)
number_chosen.current(0)

**************************************************
The new OOP code looks like this:
**************************************************
class OOP():
    def __init__(self):                # Initializer method
        # Create instance
        self.win = tk.Tk()

        # Add a title
        self.win.title("Python GUI")
        self.create_widgets()

# Button callback
def click_me(self):
    self.action.configure(text='Hello ' + self.name.get() + ' '
                        +self.number_chosen.get())
    # ... more callback methods

def create_widgets(self):
    # Create Tab Control
    tabControl = ttk.Notebook(self.win)
    tab1 = ttk.Frame(tabControl)                # Create a tab
    tabControl.add(tab1, text='Tab 1')          # Add the tab
    tab2 = ttk.Frame(tabControl)                # Create second tab
    tabControl.add(tab2, text="Tab 2")         # Add second tab
    # Pack to make visible
    tabControl.pack(expand=1, fill="both")
```