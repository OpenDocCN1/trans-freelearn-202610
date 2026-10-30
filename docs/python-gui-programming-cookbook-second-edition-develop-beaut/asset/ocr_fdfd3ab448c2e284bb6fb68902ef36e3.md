![](bbox=[0.075, 0.112, 0.155, 0.172])

Note how the cursor now defaults to residing inside the text Entry box.

We can also disable widgets. To do that, we will set a property on the widget. We can make the button disabled by adding this one line (37 below) of Python code to create the button:

```
34 # Adding a Button
35 action = ttk.Button(win, text="Click Me!", command=click_me)
36 action.grid(column=1, row=1)
37 action.configure(state='disabled')    # Disable the Button Widget
38 
39 name_entered.focus()        # Place cursor into name Entry
```

After adding the preceding line of Python code, clicking the button no longer creates any action:

![](bbox=[0.342, 0.452, 0.605, 0.548])

# How it works...

This code is self-explanatory. We set the focus to one control and disable another widget. Good naming in programming languages helps to eliminate lengthy explanations. Later in this book, there will be some advanced tips on how to do this while programming at work or practicing our programming skills at home.

# There's more...

Yes. This is only the first chapter. There is much more to come.