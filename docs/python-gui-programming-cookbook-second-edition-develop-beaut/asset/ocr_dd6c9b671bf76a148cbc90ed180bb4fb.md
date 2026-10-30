Running the preceding code results in the following screenshot:

![](bbox=[0.042, 0.145, 0.9, 0.639])

While this code works, unfortunately, it freezes our GUI. In order to fix this, we have to call the method in its own thread, as we did in the previous recipes.

Let's run our Queue method in a thread:

```
```python
# Running methods in Threads
def create_thread(self):
    self.run_thread = Thread(target=self.method_in_a_thread, args=
                             [8])
    self.run_thread.setDaemon(True)
    self.run_thread.start()

    # start queue in its own thread
    write_thread = Thread(target=self.use_queues, daemon=True)
```
```