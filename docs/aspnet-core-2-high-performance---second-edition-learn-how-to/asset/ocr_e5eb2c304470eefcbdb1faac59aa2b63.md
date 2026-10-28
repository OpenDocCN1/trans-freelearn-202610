Neither yet runs on .NET Core, but the work to port both of these projects is in progress.

Library and framework support for .NET Core and ASP.NET Core is progressing rapidly, so check http://anclafs.com/ for the latest information. Feel free to help out and contribute to this list or to any of the open source projects that need porting.

One interesting feature of MassTransit (and NSB) is support for sagas. A saga is a complex state machine that allows you to model the story of an entire workflow. Rather than defining individual messages and documenting how they all fit together, you can implicitly capture the golden path and error flows within a saga.

There is an excellent open source library called **EasyNetQ**, which makes implementing pub/sub on RabbitMQ trivial. You can read about it at http://easynetq.com/. Both the official RabbitMQ client and EasyNetQ now support .NET Core.

You could also look into using **RestBus**, which is a RabbitMQ library that supports ASP.NET Core. You can read more about it at http://restbus.org/, and it additionally supports Web API and ServiceStack.

# Summary

In this chapter, we investigated the various tools and methods used for caching and message queuing. These two techniques offer different ways of improving the performance of your system by moving data to other locations and not having one massive monolith do everything.

These are both advanced topics and difficult to cover in such a small space. Hopefully, you have been introduced to some fresh ideas that can help you with solving problems in different ways. You can also visit the author's website at https://unop.uk/ for the coverage of more topics. If you have discovered a new technology that you think will assist, you're encouraged to read the documentation and specifications for all of the implementation details.

However, before you dive in, you should understand that advanced techniques are complex and have downsides, which can reduce your development speed. In the next chapter, we'll learn about these downsides and discover approaches for managing complexities, such as microservices.