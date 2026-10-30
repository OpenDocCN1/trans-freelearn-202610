Creating queues in Python solves the problem of waiting for data to be displayed inside our GUI.

# How to do it...

In order to create queues in Python, we have to import the Queue class from the queue module. Add the following statement towards the top of our GUI module:

```
from threading import Thread
from queue import Queue
```

That gets us started.

Next, we create a queue instance:

```
def use_queues(self):
    gui_queue = Queue()                # create queue instance
    print(gui_queue)                   # print instance
```

We call the method within our button click event:

```
# Button callback
def click_me(self):
    self.action.configure(text='Hello ' + self.name.get())
    self.create_thread()
    self.use_queues()
```

![](bbox=[0.12, 0.66, 0.21, 0.78])

In the preceding code, we create a local Queue instance that is only accessible within this method. If we wish to access this queue from other places, we have to turn it into a member of our class by using the self keyword, which binds the local variable to the entire class, making it available from any other method within our class. In Python, we often create class instance variables in the __init__(self) method, but Python is very pragmatic and enables us to create those member variables anywhere in the code.