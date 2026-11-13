# Process ranks

The Android operating system tries to maintain the application running for as long as possible, but when the available memory is low, it will try to free resources in the system by killing the processes with lower importance first.

This is when process ranking comes into the picture; the Android processes are ranked in the next five categories from the higher priority to the lower priorities:

+   - **Foreground process**: This is a process that hosts an activity or service that the user is currently interacting with: a service started in the foreground or service running its life cycle callbacks
+   - **Visible process**: This is a process that hosts a paused activity or service bounded to a visible activity
+   - **Service process**: This is a process that hosts a service not bound to a visible activity
+   - **Background process**: This is a process that hosts a non-visible activity; all background processes are sorted over a **Least-Recently-Used** (LRU) list, therefore, the most recently used processes are the last killed processes when they have the same rank
+   - **Empty process**: This is a process used to cache inactive Android components and to improve any component startup time

When the system reaches a point that it needs to release resources, the processes available to be killed will be sorted, taking into account the process rank, last used processes, and components running.