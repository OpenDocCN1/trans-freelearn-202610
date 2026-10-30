# # How to use queues

A Python queue is a data structure that implements the first-in-first-out paradigm, basically working like a pipe. You shovel something into the pipe on one side and it falls out on the other side of the pipe.

The main difference between this queue shoveling and shoveling mud into physical pipes is that, in Python queues, things do not get mixed up. You put one unit in, and that unit comes back out on the other side. Next, you place another unit in (say, for example, an instance of a class), and this entire unit will come back out on the other end as one integral piece.

It comes back out at the other end in the exact order we inserted code into the queue.

![](bbox=[0.076, 0.372, 0.155, 0.432])

A queue is not a stack where we push and pop data. A stack is a **Last In First Out (LIFO)** data structure.

Queues are containers that hold data being fed into the queue from potentially different data sources. We can have different clients providing data to the queue whenever those clients have data available. Whichever client is ready to send data to our queue sends it and we can then display this data in a widget or send it forward to other modules.

Using multiple threads to complete assigned tasks in a queue is very useful when receiving the final results of processing and displaying them. The data is inserted at one end of the queue and then comes out of the other end in an ordered fashion, **First In First Out (FIFO)**.

Our GUI might have five different button widgets such that each kicks off a different task, which we want to display in our GUI in a widget (for example, a `ScrolledText` widget). These five different tasks take a different amount of time to complete.

Whenever a task has completed, we immediately need to know this and display this information in our GUI. By creating a shared Python queue and having the five tasks write their results to this queue, we can display the result of whichever task has been completed immediately, using the FIFO approach.

# ## Getting ready

As our GUI is ever-increasing in its functionality and usefulness, it starts to talk to networks, processes, and websites, and will eventually have to wait for data to be made available for the GUI to represent.