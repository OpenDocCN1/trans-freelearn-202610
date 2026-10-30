```
#-------------------------------------------------------------------
def _destroyWindow():
    root.quit()
    root.destroy()
#-------------------------------------------------------------------
root = tk.Tk()
root.withdraw()
root.protocol('WM_DELETE_WINDOW', _destroyWindow)
#-------------------------------------------------------------------
canvas = FigureCanvasTkAgg(fig, master=root)
canvas._tkcanvas.pack(side=tk.TOP, fill=tk.BOTH, expand=1)
#-------------------------------------------------------------------
root.update()
root.deiconify()
root.mainloop()
```

Running the preceding code results in the following chart:

![](bbox=[0.03, 0.41, 0.9, 0.7])

In the first line of code, after the import statements, we create an instance of a Figure object. Next, we add subplots to this figure by calling add_subplot(211). The first number in 211 tells the figure how many plots to add, the second number determines the number of columns, and the third tells the figure the order in which to display the plots.

This might become clearer by giving an example based on the Matplotlib website:

```
from matplotlib.figure import Figure
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
import tkinter as tk
#-------------------------------------------------------------------
fig = Figure(figsize=(12, 8), facecolor='white')
```