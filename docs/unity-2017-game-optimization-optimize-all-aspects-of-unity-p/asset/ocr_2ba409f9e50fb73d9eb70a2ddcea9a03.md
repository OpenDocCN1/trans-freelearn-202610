# # Masterful Memory Management

Memory efficiency is an important element of performance optimization. It’s possible for
games of limited scope, such as hobby projects and prototypes, to get away with ignoring
memory management; they will tend to waste a lot of resources, and potentially leak
memory, but this won’t be a problem if we limit its exposure to friends and coworkers.
However, anything we want to release professionally needs to take this subject seriously.
Unnecessary memory allocations lead to poor user experience due to excessive garbage
collection (costing precious CPU time), and memory leaks which will lead to crashes. None
of these situations are acceptable in modern game releases.

Using memory efficiently with Unity requires a solid understanding of the underlying
Unity Engine, Mono platform, the C# language. Also, if we’re making use of the new
IL2CPP scripting backend then it would be wise to become familiar with its inner workings.
This can be a bit of an intimidating place for some developers since many pick Unity3D for
their game development solution primarily to avoid the kind of low-level work that comes
from Engine development and memory management. We’d prefer to focus on higher-level
concerns related to gameplay implementation, level design, and art asset management, but,
unfortunately, modern computer systems are complex tools and ignoring low-level
concerns for too long could potentially lead to disaster.

Understanding what is happening with memory allocations and C# language features, how
they interact with the Mono platform and how Mono interacts with the underlying Unity
Engine are absolutely paramount to making high-quality, efficient script code. So, in this
chapter, you will learn about all of the nuts and bolts of the underlying Unity Engine: the
Mono platform, the C# language, IL2CPP, and the .NET Framework.