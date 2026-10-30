The first parameter is the title and the second is the text displayed in the pop-up message box. By adding an empty pair of single or double quotes, followed by a comma, we can move our text from the title into the pop-up message box.

We still need a title and we definitely want to get rid of this unnecessary second window. The second window is caused by a Windows event loop. We can get rid of it by suppressing it.

Add the following code:

```
from tkinter import messagebox as msg
from tkinter import Tk
root = Tk()
root.withdraw()
msg.showinfo('', 'Python GUI created using tkinter:\nThe year is 2017')
```

Now, we have only one window. The withdraw() function removes the debug window that we are not interested in having floating around:

GUI_independent_msg_one_window.py

![](bbox=[0.25, 0.508, 0.695, 0.72])

In order to add a title, all we have to do is place some string into our empty first argument.