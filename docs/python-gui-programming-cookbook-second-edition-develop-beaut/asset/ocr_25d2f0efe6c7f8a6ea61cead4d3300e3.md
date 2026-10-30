# Threads and Networking

By turning the local variable into a member, we can then check if the thread is still running by calling `isAlive` on it from another method:

```
```python
# Running methods in Threads
def create_thread(self):
    self.run_thread = Thread(target=self.method_in_a_thread, args=
                             [8])
    self.run_thread.start()
    print(self.run_thread)
    print('createThread():', self.run_thread.isAlive())
```
```

In the preceding code, we have elevated our local `run_thread` variable to a member of our class. This enables us to access the `self.run_thread` variable from any method in our class.

It is achieved like this:

```
```python
def method_in_a_thread(self, num_of_loops=10):
    for idx in range(num_of_loops):
        sleep(1)
        self.scrol.insert(tk.INSERT, str(idx) + '\n')
    sleep(1)
    print('method_in_a_thread():', self.run_thread.isAlive())
```
```

When we click the button and then exit the GUI, we can see that the print statements in the `create_thread` method were printed, but we do not see the second print statement from `method_in_a_thread`.

Consider the following output:

![](bbox=[0.338, 0.624, 0.714, 0.781])