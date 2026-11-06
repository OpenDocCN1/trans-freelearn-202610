Ironically, there’s now a counterpoint to that joke:

> There’s 2 hard problems in computer science: we only have one joke and it’s not funny.
> 
> *Phillip Scott Bowden*

The use of caching (e.g., in I/O operations) is crucial to get good performance out of slow devices, but it’s also applicable at other layers of distributed systems.

# Little’s law (throughput versus latency)

Performance is multi-dimensional. Sometimes the goal is to handle a high volume of operations (throughput), while other times, the priority is to minimize the delay between the start and completion of a single operation (latency). In many cases, both are equally important.

You can increase concurrency to improve throughput (e.g., using multithreading, asynchronous I/O, or non-blocking event loops). However, these techniques might introduce queuing delays that affect latency. Having too many concurrent operations can push a system into an inefficient state. It’s important to first identify your performance target and then optimize accordingly. For high throughput, maximize resource utilization (CPU, network, and disk) with concurrency. For low latency, minimize waiting and depth of processing. Balancing these is an art, guided by analysis and benchmarks.

In summary, performance engineering should be data-driven, using profilers and monitors to identify bottlenecks. It should be informed by both classic insights (such as algorithmic complexity and Amdahl’s law) and modern techniques (such as parallel computing, vectorization, and distributed load balancing). Focus optimization efforts where they yield the most benefit, and always verify the impact with measurements.