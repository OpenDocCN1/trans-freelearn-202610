# # Profiling memory

There are two issues we are concerned about when it comes to memory management: how
much we're consuming, and how often we're allocating new blocks. Let's cover each of
these topics separately.

# # Profiling memory consumption

We do not have direct control over what is going on in the Native Domain since we don't
have the Unity Engine source code and hence can't add any code that will interact with it
directly. We can, however, control it indirectly by means of various script-level functions
that serve as interaction points between Managed and Native Code. There are technically a
variety of memory allocators available, which are used internally for things such as
GameObjects, Graphics objects, and the Profiler, but these are hidden behind the Native-
Managed Bridge.

However, we can observe how much memory has been allocated and reserved in this
Memory Domain via the **Memory Area** of the **Profiler** window. Native memory allocations
show up under the values labeled **Unity**, and we can even get more information using
**Detailed Mode** and sampling the current frame:

| Used Total: 101.2 MB | Unity: 68.1 MB | Mono: 7.8 MB | GfxDriver: 15.8 MB | FMOD: 1.3 MB | Video: 224 B | Profiler: 9.5 MB |
|---|---|---|---|---|---|---|
| Reserved Total: 241.4 MB | Unity: 199.0 MB | Mono: 10.7 MB | GfxDriver: 15.8 MB | FMOD: 1.3 MB | Video: 224 B | Profiler: 16.0 MB |
| Total System Memory Usage: 0.78 GB | | | | | | |

Under the **Scene Memory** section of **Breakdown View**, we can observe that
`MonoBehaviour` objects always consume a constant amount of memory, regardless of their
member data. This is the memory consumed by the Native representation of the object.

Note that memory consumption in **Edit Mode** is always wildly different to
that of a stand-alone version due to various debugging and editor hook
data being applied. This adds a further incentive to avoid using **Edit**
**Mode** for benchmarking and instrumentation purposes.

We can also use the `Profiler.GetRuntimeMemorySize()` method to get the Native
memory allocation size of a particular object.

Managed object representations are intrinsically linked to their Native representations. The
best way to minimize our Native memory allocations is to simply optimize our Managed
memory usage.