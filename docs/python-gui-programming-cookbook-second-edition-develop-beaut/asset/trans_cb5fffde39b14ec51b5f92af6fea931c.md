

## 线程与网络

通过将局部变量转为成员变量，我们就可以从另一个方法中调用 `isAlive` 来检查线程是否仍在运行：

```python
# Running methods in Threads
def create_thread(self):
    self.run_thread = Thread(target=self.method_in_a_thread, args=[8])
    self.run_thread.start()
    print(self.run_thread)
    print('createThread():', self.run_thread.isAlive())
```

在前面的代码中，我们将局部 `run_thread` 变量提升为类的成员。这使我们能够从类中的任何方法访问 `self.run_thread` 变量。

实现方式如下：

```python
def method_in_a_thread(self, num_of_loops=10):
    for idx in range(num_of_loops):
        sleep(1)
        self.scrol.insert(tk.INSERT, str(idx) + '\n')
    sleep(1)
    print('method_in_a_thread():', self.run_thread.isAlive())
```

当我们点击按钮然后退出 GUI 时，可以看到 `create_thread` 方法中的打印语句已被打印，但我们看不到来自 `method_in_a_thread` 的第二个打印语句。

考虑以下输出：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_196_0.png)

我们可以通过在启动线程前调用 `setDaemon(True)` 方法来实现：

```python
# Running methods in Threads
def create_thread(self):
    self.run_thread = Thread(target=self.method_in_a_thread, args=[8])
    self.run_thread.setDaemon(True)
    self.run_thread.start()
    print(self.run_thread)
```

现在当我们点击按钮并在线程尚未完成其分配任务时退出 GUI，我们就不会再收到任何错误：

GUI_multiple_threads_stopping_a_thread.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_198_0.png)

### 如何工作...

虽然有 `start` 方法让线程运行，但令人惊讶的是并没有真正等价的 `stop` 方法。

在本示例中，我们在线程中运行一个方法，该方法向我们的 ScrolledText 小部件打印数字。

当我们退出 GUI 时，我们不再关心曾经向小部件打印的线程，因此通过将线程设为守护线程，我们可以干净地退出 GUI。

### 如何使用队列

Python 队列是一种实现先进先出范式的数据结构，基本上像管道一样工作。你在一端将东西铲入管道，它就会从另一端掉出来。

这种队列铲运与将泥浆铲入物理管道之间的主要区别在于，在 Python 队列中，东西不会被混淆。你放入一个单元，这个单元就会在另一端原样返回。接下来，你再放入另一个单元，例如一个类的实例，这个完整的单元会作为一个整体从另一端返回。

它在另一端返回的顺序与我们插入代码到队列的顺序完全一致。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_199_0.png)

队列不是我们推送和弹出数据的栈。栈是一种 **后进先出 (LIFO)** 数据结构。

队列是容纳从潜在不同数据源馈入队列的数据的容器。我们可以让不同的客户端在有数据可用时随时向队列提供数据。无论哪个客户端准备好向我们的队列发送数据，它都会发送，我们就可以在小部件中显示这些数据或将其转发到其他模块。

使用多个线程完成队列中分配的任务在接收最终处理结果并显示它们时非常有用。数据从队列的一端插入，然后以有序的方式从另一端出来，**先进先出 (FIFO)**。

我们的 GUI 可能有五个不同的按钮小部件，每个按钮触发不同的任务，我们希望在 GUI 的小部件中显示这些任务，例如 `ScrolledText` 小部件。这五个不同任务完成所需的时间各不相同。

每当任务完成时，我们需要立即知道并在 GUI 中显示此信息。通过创建一个共享的 Python 队列并让五个任务将其结果写入该队列，我们可以使用 FIFO 方法立即显示已完成的任务的结果。

#### 准备工作

随着 GUI 的功能和实用性不断增加，它开始与网络、进程和网站通信，并最终必须等待数据可用以供 GUI 表示。在 Python 中创建队列解决了等待数据在 GUI 内部显示的问题。

#### 如何操作...

为了在 Python 中创建队列，我们必须从 queue 模块导入 Queue 类。在 GUI 模块顶部添加以下语句：

```python
from threading import Thread
from queue import Queue
```

这就开始了。

接下来，我们创建一个队列实例：

```python
def use_queues(self):
    gui_queue = Queue()                # create queue instance
    print(gui_queue)                   # print instance
```

我们在按钮点击事件中调用该方法：

```python
# Button callback
def click_me(self):
    self.action.configure(text='Hello ' + self.name.get())
    self.create_thread()
    self.use_queues()
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_200_0.png)

在前面的代码中，我们创建了一个仅在此方法内可访问的局部 Queue 实例。如果我们希望从其他地方访问此队列，就必须通过使用 self 关键字将其转为类的成员，这会将局部变量绑定到整个类，使其可从类中的任何其他方法访问。在 Python 中，我们通常在 `__init__(self)` 方法中创建类实例变量，但 Python 非常务实，允许我们在代码中的任何位置创建这些成员变量。

现在我们有一个队列实例。我们可以通过打印它来证明它有效：

```
Hi, how are you?
<Thread(Thread-1, started daemon 6432)>
createThread(): True
<queue.Queue object at 0x0000023C005534A8>
method_in_a_thread(): True
```

为了将数据放入队列，我们使用 put 命令。为了从队列中取出数据，我们使用 get 命令：

```python
# Create Queue instance
def use_queues(self):
    gui_queue = Queue()
    print(gui_queue)
    gui_queue.put('Message from a queue')
    print(gui_queue.get())
```

运行修改后的代码后，消息首先被放入 Queue，然后从 Queue 取出，最后打印到控制台：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_201_0.png)

我们可以将许多消息放入 Queue：

```python
# Create Queue instance
def use_queues(self):
    gui_queue = Queue()
    print(gui_queue)
    for idx in range(10):
        gui_queue.put('Message from a queue: ' + str(idx))
    print(gui_queue.get())
```

我们已将十条消息放入 Queue，但我们只取出第一条。其他消息仍在 Queue 中，等待以 FIFO 方式取出：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_202_0.png)

为了取出所有已放入 Queue 的消息，我们可以创建一个无限循环：

GUI_queues_put_get_loop_endless.py

```python
# Create Queue instance
def use_queues(self):
    gui_queue = Queue()
    print(gui_queue)
    for idx in range(10):
        gui_queue.put('Message from a queue: ' + str(idx))
    while True:
        print(gui_queue.get())
```

运行前面的代码会得到以下截图：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_203_0.png)

虽然这段代码可以工作，但不幸的是，它会冻结我们的 GUI。为了修复这个问题，我们必须在自己的线程中调用该方法，就像在之前的示例中所做的那样。

让我们在线程中运行我们的 Queue 方法：

```python
# Running methods in Threads
def create_thread(self):
    self.run_thread = Thread(target=self.method_in_a_thread, args=[8])
    self.run_thread.setDaemon(True)
    self.run_thread.start()

    # start queue in its own thread
    write_thread = Thread(target=self.use_queues, daemon=True)
```

