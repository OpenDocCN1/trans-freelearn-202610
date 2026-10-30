```
```python
outputFrame.grid(column=0, row=2, sticky=e, padx=8)
iframe=None
if y:chk2=tkinter.Checkbutton(lFrame,text="Enabled",variable=chVarUn)
wE='WE'
if y==x:PRINTME(x)
lFrame=tkinter.ttk.LabelFrame(win,text="Spaghetti")
```
```

Consider the following example of good code::

```
```python
#==============================================================
# Adding a LabelFrame, Textbox (Entry) and Combobox
#==============================================================
lFrame = ttk.LabelFrame(win, text="Python GUI Programming Cookbook")
lFrame.grid(column=0, row=0, sticky='WE', padx=10, pady=10)

#==============================================================
# Using a scrolled Text control
#==============================================================
outputFrame = ttk.LabelFrame(win, text=' Type into the scrolled text
                                      control: ')
outputFrame.grid(column=0, row=2, sticky='E', padx=8)
```
```

It does not contain unnecessary variable assignments and neither does it have a `print` function that does not do the debugging one might expect it to when reading the code.

For example, consider the following spaghetti code:

```
```python
def PRINTME(me):print(me)
x=y=z=1
e='E'
WE='WE'
scr.grid(column=1,row=1,sticky=WE)
wE='WE'
if y==x:PRINTME(x)
lFrame.grid(column=0,row=0,sticky=WE,padx=10,pady=10)
PRINTME(z)
try: win.mainloop()
except: PRINTME(x)
chk2.deselect()
if y==x:PRINTME(x)
```
```

Good code has none of the instances mentioned for the spaghetti code.

The `import` statements only import the required modules. They are not cluttered throughout the code. There are no duplicate `import` statements. There is no `import *` statement.