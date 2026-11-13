# # ART runtime

The ART runtime is a new version of the DVM and was introduced to improve
the runtime performance and memory consumption. The new runtime was
introduced in Android 4.4 KitKat as an experimental runtime, and since the
Android 5.0 Lollipop, it has become the main Android runtime.

This new runtime, making use of the **ahead-of-time (AOT)** compilation, brings
new app-performance optimizations on startup time and application execution.
The AOT, as opposed to DVM JIT (Just in Time), compiles the dex files during
the installation time using the device dex2oat tool. The compiled code generated
from the dex2oat tool generates system-dependent code for the target device and
removes the delay introduced by the JIT compilation during each application
execution.

The AOT compiler also reduces the number of processor cycles used by the
application as it removes the time spent by the JIT compiler to convert the code
into machine code, and then uses less battery power to run the application.

One of the drawbacks of the AOT compilation is the larger memory footprint in
comparison with the JIT used by DVM.

With the new runtime, some improvements were also introduced in the memory
allocation and on **Garbage Collection (GC)**, resulting in a more responsive UI
and better application experience.