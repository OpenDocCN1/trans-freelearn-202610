# How it works...

In this recipe, we prepared our GUI to use threads by first increasing the GUI size so we can better see the results printed to the `ScrolledText` widget.

We then imported the `Thread` class from the Python `threading` module.

Next we created a method that we call in a thread from within our GUI.

# Starting a thread

This recipe will show us how to start a thread. It will also demonstrate why threads are necessary to keep our GUI responsive during long-running tasks.

# Getting ready

Let's first see what happens when we call a function or a method of our GUI that has some `sleep` associated with it without using threads.

![](bbox=[0.06, 0.54, 0.14, 0.62])

We are using `sleep` here to simulate a real-world application that might have to wait for a web server or database to respond, a large file transfer, or complex computations to complete its task.
`sleep` is a very realistic placeholder and shows the principle involved.

Adding a loop into our button callback method with some sleep time results in our GUI becoming unresponsive and, when we try to close the GUI, things get even worse:

GUI_multiple_threads_sleep_freeze.py

```
# Button callback
def click_me(self):
    self.action.configure(text='Hello ' + self.name.get() + ' '
        + self.number_chosen.get())
    # Non-threaded code with sleep freezes the GUI
    for idx in range(10):
        sleep(5)
        self.scrol.insert(tk.INSERT, str(idx) + '\n')
```