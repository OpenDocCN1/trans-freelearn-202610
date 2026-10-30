We still need to import the tkinter package itself, but we have to specify that we now want to also use ttk from the tkinter package.

ttk stands for themed tk. It improves our GUI's look and feel.

Line 19 adds the label to the GUI, just before we call mainloop.

We pass our window instance into the ttk.Label constructor and set the text property. This becomes the text our Label will display.

We also make use of the grid layout manager, which we'll explore in much more depth in Chapter 2, Layout Management.

Note how our GUI suddenly got much smaller than in the previous recipes.

The reason why it became so small is that we added a widget to our form. Without a widget, the tkinter package uses a default size. Adding a widget causes optimization, which generally means using as little space as necessary to display the widget(s).

If we make the text of the label longer, the GUI will expand automatically. We will cover this automatic form size adjustment in a later recipe in Chapter 2, Layout Management.

# There's more...

Try resizing and maximizing this GUI with a label and watch what happens.

# Creating buttons and changing their text property

In this recipe, we will add a button widget, and we will use this button to change a property of another widget that is a part of our GUI. This introduces us to callback functions and event handling in a Python GUI environment.