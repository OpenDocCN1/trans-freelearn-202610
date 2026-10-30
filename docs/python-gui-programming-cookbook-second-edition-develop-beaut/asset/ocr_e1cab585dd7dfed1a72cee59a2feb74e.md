Let's align the entry in column 0 to the left:

```
# Adding a Textbox Entry widget
name = tk.StringVar()
name_entered = ttk.Entry(mighty, width=12, textvariable=name)
name_entered.grid(column=0, row=1, sticky=tk.W)      # align left/West
```

Now, both the label and the entry are aligned towards the West (left):

GUI_embed_frames_align_entry_west.py

![](bbox=[0.22, 0.335, 0.712, 0.63])

In order to separate the influence that the length of our Labels in a Frame LabelFrame has on the rest of our GUI layout, we must not place this LabelFrame into the same LabelFrame as the other widgets but assign it directly to the main GUI form (win). We will do this in the later chapters.

# Creating menu bars

In this recipe, we will add a menu bar to our main window, add menus to the menu bar, and then add menu items to the menus.