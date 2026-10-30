# How to do it...

In this chapter, we have created rows and columns, which truly is a database approach to GUI design (MS Excel does the same). We hardcoded the first rows, but then we forgot to give the next row a specification of where we wish it to reside.

Tkinter did fill this in for us without us even noticing.

Here is what we did in our code:

```
# Using a scrolled Text control
scrol_w = 30
scrol_h = 3
scr = scrolledtext.ScrolledText(mighty, width=scrol_w, height=scrol_h, wrap=tk.WORD)
# scr.grid(column=0, row=2, sticky='WE', columnspan=3)
scr.grid(column=0, sticky='WE', columnspan=3)            # row not specified
```

Tkinter automatically adds the missing row where we did not specify any particular row. We might not realize this.

We laid out the Entry widgets on row 1, then we forgot to specify the row for our ScrolledText widget which we reference via the scr variable, and then we added the Radiobutton widgets to be laid out in row 3.

This works nicely because tkinter automatically incremented the row position for our ScrolledText widget to use the next highest row number, which was row 2.

Looking at our code and not realizing that we forgot to explicitly position our ScrolledText widget to row 2, we might think nothing resides there.

So, we might try the following. If we set the variable curRad to use row 2, we might get an unpleasant surprise:

GUI_grid_layout.py