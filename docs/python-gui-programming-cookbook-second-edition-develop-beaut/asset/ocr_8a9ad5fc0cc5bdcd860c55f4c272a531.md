# There's more...

If we want to restrict the user to only be able to select the values we have programmed into the Combobox, we can do that by passing the state property into the constructor. Modify line 42 as follows:

GUI_combobox_widget_readonly_plus_display_number.py

```
40 ttk.Label(win, text="Choose a number:").grid(column=1, row=0)
41 number = tk.StringVar()
42 number_chosen = ttk.Combobox(win, width=12, textvariable=number, state='readonly')
43 number_chosen['values'] = (1, 2, 4, 42, 100)
44 number_chosen.grid(column=1, row=1)
45 number_chosen.current(0)
```

Now, users can no longer type values into the Combobox. We can display the value chosen by the user by adding the following line of code to our Button Click Event Callback function:

```
22 # Modified Button Click Function
23 def click_me():
24     action.configure(text='Hello ' + name.get() + ' ' +
25                         number_chosen.get())
```

After choosing a number, entering a name, and then clicking the button, we get the following GUI result, which now also displays the number selected:

![](bbox=[0.34, 0.555, 0.7, 0.65])

# Creating a check button with different initial states

In this recipe, we will add three check button widgets, each with a different initial state.

# Getting ready

This recipe extends the previous recipe, Combo box widgets.