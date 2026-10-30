```
messagebox.showinfo('Message Box', 'Hi from same Level.')

button = ttk.Button(lFrame, text="Click Me ", command=clickMe)
button.grid(column=1, row=0, sticky=tk.S)

#========================
# Start GUI
#========================
win.mainloop()
```

Running the preceding code named GUI__init.py, we get the following output:

![](bbox=[0.15, 0.31, 0.88, 0.56])

In the preceding code, we created the following function, which imports Python's message box and then uses it to display the message box dialog window:

```
def clickMe():
    from tkinter import messagebox
    messagebox.showinfo('Message Box', 'Hi from same Level.')
```

When we move the clickMe() message box code into a nested directory folder and try to import it into our GUI module, we might run into some challenges.

We have created three subfolders below where our Python module lives. We have then placed the clickMe() message box code into a new Python module, which we named MessageBox.py. This module lives in Folder3, three levels below where our Python module lives.

We need to import this MessageBox.py module in order to use the clickMe() function that this module contains.