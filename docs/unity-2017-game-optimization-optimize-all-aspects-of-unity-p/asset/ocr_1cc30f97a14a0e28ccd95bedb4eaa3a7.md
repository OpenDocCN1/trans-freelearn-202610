Note that we can hide different log level types with the buttons shown in the next screenshot. The extra logging still costs CPU and memory to execute, even though they are not being rendered, but it does allow us to filter out the junk we don't want. Although, it is often a good practice to keep all of these options enabled to verify that we're not missing anything important.

![](bbox=[0.3496, 0.2188, 0.6484, 0.3062])

# # Minimizing external distractions

This one is simple but absolutely necessary. We should double-check that there are no background processes eating away CPU cycles or consuming vast swathes of memory. Being low on available memory will generally interfere with our testing, as it can cause more cache misses, hard-drive access for virtual memory page-file swapping, and generally slow responsiveness of the application. If our application is suddenly behaving significantly worse than what we expected, double-check the system's task manager (or equivalent) for any CPU/memory/hard disk activity, which might be causing problems.

# # Targeted profiling of code segments

If our performance problem isn't resolved by the checklist mentioned previously, then we probably have a real issue on our hands that demands further analysis. The Profiler window is effective at showing us a broad overview of performance; it can help us find specific frames to investigate and can quickly inform us which `MonoBehaviour` and/or method may be causing issues. We would then need to figure out whether the problem is reproducible, under what circumstances a performance bottleneck arises, and where exactly within the problematic code block the issue is originating from.

To accomplish these, we will need to perform some profiling of targeted sections of our code, and there are a handful of useful techniques we can employ for this task. For Unity projects, they essentially fit into two categories:

+   - Controlling the Profiler from script code
+   - Custom timing and logging methods