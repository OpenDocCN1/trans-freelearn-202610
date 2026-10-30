This also works on the second tab:

![](bbox=[0.23, 0.14, 0.7, 0.52])

The import statements look as follows:

```
13  from tkinter import messagebox as msg
14  from tkinter import Spinbox
15  from time import sleep
16  import Ch04_Code.ToolTip as tt
17
18  GLOBAL_CONST = 42
19
20  #==============================================
21  class OOP():
22      def __init__(self):      # Initializer method
23          # Create instance
24          self.win = tk.Tk()
25
26          tt.create_ToolTip(self.win, 'Hello GUI')
27
```