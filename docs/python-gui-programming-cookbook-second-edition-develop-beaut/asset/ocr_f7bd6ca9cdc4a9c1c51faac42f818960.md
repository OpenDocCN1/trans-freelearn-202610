```
check2 = tk.Checkbutton(lFrame, text="Enabled", variable=chVarUn)
check2.deselect()
check2.grid(column=1, row=4, sticky=tk.W, columnspan=3)

#========================
# Start GUI
#========================
win.mainloop()
```

# How it works...

In this recipe, we compared spaghetti code to good code. Good code has many advantages over spaghetti code.

It has clearly commented sections.

The following is an example of spaghetti code:

```
def PRINTME(me):print(me)
import tkinter
x=y=z=1
PRINTME(z)
from tkinter import *
```

The following is an example of good code:

```
#====================
# imports
#====================
import tkinter as tk
from tkinter import ttk
```

It has a natural flow that follows how the widgets get laid out in the main GUI form.

In the spaghetti code, the bottom LabelFrame gets created before the top LabelFrame, and it is intermixed with an import statement and some widget creation.

Consider the following example of spaghetti code:

```
import tkinter.scrolledtext
outputFrame=tkinter.ttk.LabelFrame(win,text=' Type into the scrolled text control: ')
scr=tkinter.scrolledtext.ScrolledText(outputFrame,width=scrolW,height=scrolH,wrap=tkinter.WORD)
e='E'
scr.grid(column=1,row=1,sticky=WE)
```