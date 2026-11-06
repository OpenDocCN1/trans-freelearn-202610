
## Inside the BEAM

The Erlang VM is commonly known as **BEAM (Bogdan/Björn's Erlang Abstract Machine)**. It was designed to run highly-reliable systems that usually have many nines of availability, and are pretty much always able to respond to client requests. We will now look at some of the design decisions behind the BEAM, and how they enable the creation and deployment of such systems.

A process is the unit of concurrent execution inside the BEAM. As we will see throughout this chapter, they are the building block that enables the creation of scalable, robust, fault-tolerant systems. Before diving into some runtime considerations for using processes, let's explore how processes interact with one another.

There's much discussion about whether the BEAM is a legitimate implementation of the actor model, as described by Carl Hewitt in the 1970s. Robert Virding, a co-creator of Erlang, has repeatedly stated that, while developing Erlang, they arrived at an implementation that resembles the actor model by accident, only getting to know the actor model itself much later. Regardless, the term *actor* isn't widely used in the Elixir/Erlang community—we mostly just use the term *process*.

Each process in the BEAM maintains its own private state, which can never be shared with another process. Processes communicate via asynchronous message passing, and each one has its own mailbox, which stores the messages sent to that process. Upon receiving a message, a process can either just reply to it by performing a certain computation, sending messages to other processes, spawning new processes, or changing its own internal state. Queuing and dequeuing of messages from a mailbox are atomic, and a process handles them sequentially.

To send a message to a process, you must know its PID (a basic type, as we learned in *Chapter 1, Preparing for the Journey Ahead*) or its registered alias. A process can also send a message to itself, which will create a new message in its own mailbox, and will be handled in the future. Messages sent from one actor to another are guaranteed to maintain their order at the receiver's mailbox, but note that the interleaving of multiple messages sent by multiple processes is indeterminate on the receiver's end, and you should never rely on it.

To handle incoming messages, a process can use pattern matching. Empowered with this mechanism, we can build processes that either listen, or filter a certain kind of message, or just behave differently according to the message.

As previously stated, the message passing is asynchronous, but only from the sender perspective—when receiving a message, the process is blocked, waiting for a new message on its mailbox, as we will further explain in the next section.

While this isolated, share-nothing model of processes may raise some performance concerns, since every message exchanged has to be deep copied, it also brings some incredible benefits, such as these:

- It's easy to build concurrent applications. With no shared state, there's no need for mutexes or semaphores. Processes run concurrently (and possibly in parallel, if there are at least two CPU cores), allowing you to take full advantage of your hardware.
- Upon crashing, a process can instantly be replaced. As we will learn later in this chapter, we can have processes whose only job is to supervise other processes, and take a certain action when they go down. Again, since there's no shared state, the crash of a certain process can be resolved, in most cases, just by restarting it.
- There is no Stop-the-World garbage collection. Since each process maintains its own heap, garbage collection occurs at the process level. Moreover, when a process exits, its memory space can simply be reclaimed. Garbage collection doesn't stop the whole VM, which means that your processes may continue to reply to requests when a certain process is stopped for garbage collection.
- It is simple to distribute and scale horizontally. Given that processes are isolated, you can scale your application by adding more computing power, which in turn adds the ability to create more processes. With the incredible work done in the BEAM, communicating with a process on your own machine or some other machine in the cluster is virtually the same.

As we previously suggested, processes inside the BEAM are very lightweight, and can be spawned or destroyed with minimal overhead. Generally, it only takes a couple of microseconds to create a new process, while its initial memory usage is between 1 and 2 KB. To put this in perspective, by default, a thread uses 2 MB just for the stack on a Linux x86 system. This allows you to have millions of processes running in a single machine.

BEAM runs on a single OS process, and then spawns threads under that process, by default, based on the number of cores available. Each thread will run a scheduler that will preemptively manage the CPU time of each process. The BEAM is a soft real-time system, and this preemptive scheduling promotes fairness and liveness.

This way, a CPU-intensive process running for a long time will not influence the performance of other processes. Imagine that you're running an HTTP server in this model, where each request is handled by a different process. If a request from a certain client is taking more time to respond to, it will have no effect on other users of the HTTP server, keeping the system live for most users.

![](img/c636f4cfb07f25f1db535995e50706ed_114_0.png)

We've stated that you can run millions of processes inside the BEAM, but that, by default, it's capped at around 256,000. You can increase this limit by passing options with the `--erl` flag when starting `elixir` or `iex`, as in: `iex --erl "+P 10000000"`. The `+P` option controls the maximum number of simultaneous processes inside the BEAM. You can check out other options at http://erlang.org/doc/man/erl.html.

Most programmers know that building a concurrent application is hard work. Frequently, making an application concurrent is not only delayed but sometimes the last resort, and is carried out when the performance requirements have no other solution. This probably happens because they know it is a source of some scary and hard-to-find bugs, and that generally this change makes programs harder to reason about.

While this is true with traditional concurrency primitives, such as threads, we want to make the case that this is not true for BEAM processes. Joe Armstrong, Erlang's co-creator, occasionally uses a very interesting expression when talking about Erlang: Concurrency-oriented programming. The philosophy is that concurrency should be baked into our programs right from the start, and that it's the basis for distribution and fault tolerance.

Well, that's enough theory! Let's now explore how to work with BEAM processes.

## Working with processes

After seeing the conceptual model of how processes work inside the BEAM, we will now see how we can work with them. Namely, we will cover how to work with processes, how to pass messages between them, and, to showcase how we can use processes to maintain state, we will build a component of our ElixirDrip app, the cache worker.

## Creating processes

You create a process by calling the Kernel.spawn/1 function, which receives an anonymous function with no arguments:

```
iex> self()
#PID<0.85.0>
iex> spawn(fn ->
...>   :timer.sleep(2000)
...>   IO.puts "I'm running in process #{inspect(self())}"
...> end)
#PID<0.91.0>
I'm running in process #PID<0.91.0>
```

In this example, we're first calling self(), which returns the PID of the current process. In this case, it returned the PID of the shell process. Then, we use the spawn function to create a new process. As previously stated, it receives an anonymous function that will run in the newly created process. The anonymous function that we're passing to spawn uses the timer Erlang library to sleep for two seconds, and then uses the puts function from the IO module to print a string to the terminal. This string prints the PID of the current process. As we can see from the preceding output, when calling self() directly from IEx, we get its PID, <0.85.0>, but the string printed to the terminal shows the <0.91.0> PID—the PID of the process created by the spawn function. Note that the return value of this function is the PID of the process that was just created, which will be important later, when we start to pass messages between processes.

Another important aspect to point out is that the caller of the spawn function resumes its work as soon as the process is created. We've introduced sleep on the anonymous function so that this behavior is easily observed. If you run this example in your IEx session, you'll see that right after calling spawn, you get your shell back—meaning that the shell process has finished creating the new process and is ready to receive input. Then, about two seconds later, you see the string we talked about earlier being printed to the screen. This means that the sleep only occurred on the new process, and the caller of spawn was unblocked earlier.

Let's now look at something a bit more interesting. In the following example, we'll pass a variable defined in the outer scope to the newly created process:

```
iex> x = 4
4
iex> spawn(fn ->
...>   x = x * x
...>   IO.puts "The number squared is #{x}"
...> end)
```
