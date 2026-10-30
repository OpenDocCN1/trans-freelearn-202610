We simulated long-running tasks by inserting a sleep statement into our code, which showed us that threads can indeed solve our problem.

# Stopping a thread

We have to start a thread to actually make it do something by calling the start() method, so intuitively, we would expect there to be a matching stop() method, but there is no such thing. In this recipe, we will learn how to run a thread as a background task, which is called a daemon. When closing the main thread, which is our GUI, all daemons will automatically be stopped as well.

# Getting ready

When we call methods in a thread, we can also pass arguments and keyword arguments to the method. We start this recipe by doing exactly that.

# How to do it...

By adding args=[8] to the thread constructor and modifying the targeted method to expect arguments, we can pass arguments to the threaded methods. The parameter to args has to be a sequence, so we will wrap our number in a Python list:

```
def method_in_a_thread(self, num_of_loops=10):
    print('Hi, how are you?')
    for idx in range(num_of_loops):
        sleep(5)
        self.scrol.insert(tk.INSERT, str(idx) + 'n')
```

In the following code, run_thread is a local variable, which we only access within the scope of the method inside which we created run_thread:

```
# Running methods in Threads
def create_thread(self):
    run_thread = Thread(target=self.method_in_a_thread, args=[8])
    run_thread.start()
```