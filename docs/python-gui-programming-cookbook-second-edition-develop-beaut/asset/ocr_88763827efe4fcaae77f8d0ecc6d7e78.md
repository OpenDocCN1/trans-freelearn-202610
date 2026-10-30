We can do this by calling the setDaemon(True) method on the thread before we start the thread:

```
# Running methods in Threads
def create_thread(self):
    self.run_thread = Thread(target=self.method_in_a_thread, args=[8])
    self.run_thread.setDaemon(True)
    self.run_thread.start()
    print(self.run_thread)
```

When we now click the button and exit our GUI while the thread has not yet completed its assigned task, we no longer get any errors:

GUI_multiple_threads_stopping_a_thread.py

![](bbox=[0.141, 0.38, 0.917, 0.63])

# How it works...

While there is a start method to make threads run, surprisingly there isn't really an equivalent stop method.

In this recipe, we are running a method in a thread, which prints numbers to our ScrolledText widget.

When we exit our GUI, we are no longer interested in the thread that used to print to our widget, so by turning the thread into a daemon, we can exit our GUI cleanly.