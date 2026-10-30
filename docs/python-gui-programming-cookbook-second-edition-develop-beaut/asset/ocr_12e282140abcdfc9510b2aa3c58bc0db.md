Running the code will result in the GUI looking as follows:

![](bbox=[0.274, 0.141, 0.778, 0.419])

Uncomment line 111 and notice the different alignment of LabelFrame.

We can easily align the labels vertically by changing our code, as shown in the next screenshot:

```
```python
113  # Place labels into the container element
114  ttk.Label(buttons_frame, text="Label1").grid(column=0, row=0)
115  ttk.Label(buttons_frame, text="Label2").grid(column=0, row=1)
116  ttk.Label(buttons_frame, text="Label3").grid(column=0, row=2)
117
118  for child in buttons_frame.winfo_children():
```
```

Note that the only change we had to make was in the column and row numbering.