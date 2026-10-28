
从某种程度上说，ZeroMQ 会尝试从外部错误中恢复。例如，它会尝试以智能方式处理网络错误，甚至在合理时自动重试。尽管该框架无法处理可能发生的每一个错误，但任何确实导致调用无法成功完成的错误，很可能都需要发起调用的进程来处理。即使 ZeroMQ 其他什么都不做，仅凭这一特性就值得使用。

如果由于某种原因无法成功，在底层，这意味着每个 ZeroMQ 调用都会返回一个状态码或导致断言失败。ZeroMQ 的 Python 包装器往往会针对任何类型的错误事件抛出异常。下表总结了这些异常：

| 异常 | 条件 |
|---|---|
| ZMQError | 包装一个 ZeroMQ 错误编号 |
| ZMQVersionError | 当链接的 libzmq 版本未提供某个特性时抛出 |
| Again | ZeroMQ EAGAIN 错误的包装器 |
| ContextTerminated | ZeroMQ ETERM 错误的包装器 |
| NotDone | 等待消息时超时 |
| ZMQBindError | 当套接字无法绑定到端口时 |

完整细节可以在 `https://pymq.readthedocs.org/en/latest/api/index.html` 找到。每次对 ZeroMQ 的调用都应该包装在 try-catch 块中，并且鼓励大量使用断言。然而，尽管错误处理在生产环境中很重要，但它往往会扰乱原本简洁的代码。所有示例都将省略错误处理：

- **第 14 行：** 接收请求是一个阻塞语句。该进程将无限期等待先前指定端口上的连接。可以指定一个限制，使等待超时。
- **第 21 行：** 收到消息并完成（一些最少限度的）处理后，可以将响应发送回发起连接的进程。在底层，ZeroMQ 发送的是字节，而不是 Python 字符。如果客户端和服务器都是 Python（且可能是同一版本），以这种方式发送字符串应该没有问题。

跨语言和跨平台应用程序可能希望使用以下方式：

```
socket.send(b"World")
```

而不是使用下面这行代码：

```
socket.send("World")
```

# 发布者

这是一个发布者。它使用 PUB 套接字将时间和天气发送给任意数量的订阅者：

```
```python
import zmq
import random

context = zmq.Context()
socket = context.socket(zmq.PUB)
socket.bind("tcp://*:5678")

while True:
    topic = "load"
    socket.send(topic + " " + str(random.random()))

    topic = "weather"
    messagedata = "dark and stormy night"
    socket.send(topic + " " + messagedata)

    time.sleep(1)
```
```

# 订阅者

该订阅者使用 SUB 套接字来监听上述发布者。但它只对天气感兴趣：

```
```python
import zmq

context = zmq.Context()
socket = context.socket(zmq.SUB)
socket.connect("tcp://localhost:5678")
socket.setsockopt_string(zmq.SUBSCRIBE, "weather")

while True:
    wtr = socket.recv_string()
    print("the weather is: " + wtr)
```
```

# 广播

使用 MPI 时，广播似乎并不适用：毕竟，每个进程都运行相同的代码，那么一个进程怎么能知道另一个进程不知道的信息呢？一个不对称的例子来自 I/O 的使用。考虑一个程序，它接收用户输入，然后在每个进程中根据该输入执行操作。如果每个进程都独立地向用户请求输入，那将是疯狂的。0 号进程（rank 0）应该收集输入，然后将其发送给所有其他进程，例如：

```
```python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

if rank == 0:
    data = input("Please enter random number seed")
else:
    data = None

# mpi4py wants to send an object, so we will leave the
# input in that format
data = comm.bcast(data, root=0)
```
```

# 归约

当所有进程执行各自独立的操作时，将它们的计算结果合并起来可能很有用。例如，在计算函数的黎曼积分时，将工作分配到多个进程上可能会高效得多。然后所得结果就是每个进程中计算出的值之和，例如：

```
```python
import numpy

import sys
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

# integrate from in [0, 4] using 8 rectangles
a = 0
b = 4
```
```

## ZeroMQ 与 IPython

ZeroMQ 不仅仅是一个用于并行环境的消息库：IPython 已经转向使用 ZeroMQ 作为其内部消息传递基础设施。在本节中，我们将从 ZeroMQ 底层机制的角度，提供有关 IPython 运行的更多细节。

为此，我们将介绍一些额外的套接字类型，描述它们如何交互以支持 IPython 的某些并行组件，最后介绍一些使用这些组件的用例。虽然本节无法涵盖 IPython 并行架构的所有细节，但希望这些示例能让用户对底层机制有一个基本的了解。

### ZeroMQ 套接字类型

ZeroMQ 支持大量不同类型的套接字，截至目前（版本 4.2.0）共有 14 种。上一节概述了一些重要的类型，但要讨论 IPython 架构，至少需要对一些高级套接字类型有初步了解：

- **DEALER：** 这类似于 REQ 套接字，但它是异步的。每条发送的消息都会以轮询（round-robin）方式在所有已连接对等节点间发送，每条接收的消息都会从所有已连接对等节点公平排队。它与 ROUTER、REP 和 DEALER 套接字兼容。
- **ROUTER：** 这类似于 REP 套接字，但它是异步的。它会跟踪所有已连接的对等节点。路由器发送的每条消息都包含消息来源的身份标识。这使得套接字能够根据消息来源进行路由。当 ROUTER 连接了多个 REQ 和 REP 套接字时，需要将消息发送到正确的端点。

### IPython 组件

*第 3 章“迈向 IPython 并行计算”* 概述了 IPython 的主要架构组件。在本节中，我们将提供更多细节，重点介绍每个组件如何使用消息传递来执行其任务。

ZeroMQ 追求最小化配置：客户端和引擎只需知道 Hub 的 ROUTER 套接字的 IP 地址和端口即可连接。一旦建立了这个初始连接，建立进一步连接所需的任何信息都会发送过去。此外，客户端还可以利用此连接向 Hub 查询关于引擎的状态信息。这样就形成了一个简单的连接架构：客户端和引擎都使用 DEAL 套接字，而 Hub 使用 ROUT 套接字：

![](img/8628b86fcc4f22d859c0104c2ec1d15f_140_0.png)

# 心跳

Hub 还负责检查哪些引擎仍在响应。为此，它会定期向每个引擎发送一条消息。然后引擎会以相同的消息进行响应，并带有前缀表示哪个引擎作出了响应。如果某个引擎没有响应，Hub 将尝试终止它。

