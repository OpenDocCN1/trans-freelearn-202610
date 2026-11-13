
## Android process model

Android is a multiuser, multitasking system that can run multiple applications in parallel, where all the applications attempt to acquire CPU time to execute its job.

Each application runs independently on an isolated Linux process cloned from the Zygote process, and by default, all the Android components run within the same process with the same name as the application package specified in Android Application Manifest (AAM).

The Linux kernel will fairly allocate small amounts of CPU time for application execution called CPU time slices. This time-slicing approach means that even a single-processor device can appear to be actively working in more than one application at the same time, when in fact, each application is taking very short turns on the CPU.

## Process ranks

The Android operating system tries to maintain the application running for as long as possible, but when the available memory is low, it will try to free resources in the system by killing the processes with lower importance first.

This is when process ranking comes into the picture; the Android processes are ranked in the next five categories from the higher priority to the lower priorities:

- **Foreground process**: This is a process that hosts an activity or service that the user is currently interacting with: a service started in the foreground or service running its life cycle callbacks
- **Visible process**: This is a process that hosts a paused activity or service bounded to a visible activity
- **Service process**: This is a process that hosts a service not bound to a visible activity
- **Background process**: This is a process that hosts a non-visible activity; all background processes are sorted over a **Least-Recently-Used** (LRU) list, therefore, the most recently used processes are the last killed processes when they have the same rank
- **Empty process**: This is a process used to cache inactive Android components and to improve any component startup time

When the system reaches a point that it needs to release resources, the processes available to be killed will be sorted, taking into account the process rank, last used processes, and components running.

## Process sandboxing

The Android application always runs under a unique **Linux user ID (UID)** assigned to the application during the application installation so that the process runs on a sandboxed environment, which by default, isolates your data and code execution from other apps.

In some cases, a user could explicitly be required to share the UID with another application to have access to their data:

| USER | PID | PPID | VSIZE | RSS | PC | NAME |
|---|---|---|---|---|---|---|
| root | 319 | 1 | 1537236 | 31324 | S | zygote |
| ... | | | | | | |
| u0_a221 | 5993 | 319 | 1731636 | 41504 | S | com.whatsapp |
| u0_a96 | 3018 | 319 | 1640252 | 29540 | S | com.dropbox.android |
| u0_a255 | 4892 | 319 | 1583828 | 34552 | S | com.accuweather.android... |

The preceding table that results from running the `adb shell ps` command in the computer with Android SDK Table is a list of Android running processes.

The first column shows the **user identifier (UID)** assigned at the time of installation, the second column is the **process ID (PID)**, the third column shows the **parent process ID (PPID)** that for Android applications is the Zygote process, and the last column shows the application package.

From this list, we can assure that the WhatsApp application is running under the user ID u0_a221 with the process ID 5993 and the parent process is the Zygote process with the PID 319.

## Android thread model

Within an Android process, there may be many threads of execution. Each thread is a separate sequential flow of control within the overall program—it executes its instructions in order, one after the other, and they also share allocated slices of CPU time managed by the operating system task scheduler.

While the application process is started by the system and prevented from directly interfering with data in the memory address space of other processes, the threads may be started by an application code and can communicate and share data with other threads within the same process. Apart from the shared data that all the threads share in the same process, a thread can use its own memory cache to store its data in its own memory space.

### The main thread

When the application process starts, apart from DVM housekeeping threads, the system creates a thread of execution called main. This thread, as the name explains, plays a crucial role in the application lifetime as it is the thread that interacts with the Android UI components, updating the state and their look on the device screen.

Moreover, by default, all the Android application components (Activity, Service, ContentProvider, and BroadcastReceiver) are also executed over the main thread line of execution. The following image shows the lists of threads running inside an application process with the main thread at the top of the list with a unique thread ID (TID) assigned by the system:

| ID | Tid | Status | utime | stime | Name |
|---|---|---|---|---|---|
| 1 | 30155 | Runnable | 4 | 8 | main |
| 2 | 30161 | Wait | 0 | 0 | Signal Catcher |
| 3 | 30162 | Runnable | 104 | 94 | JDWP |
| 4 | 30166 | Wait | 0 | 0 | FinalizerDaemon |
| 5 | 30165 | Wait | 0 | 0 | ReferenceQueueDaemon |
| 6 | 30164 | Runnable | 0 | 0 | Binder_1 |
| 7 | 30170 | Monitor | 0 | 0 | GCDaemon |
| 8 | 30169 | Wait | 0 | 0 | HeapTrimmerDaemon |
| 9 | 30167 | Runnable | 0 | 0 | Binder_2 |
| 10 | 30168 | Wait | 0 | 0 | FinalizerWatchdogDaemon |
| 11 | 30198 | Runnable | 4 | 5 | RenderThread |

The main thread, also known as UI Thread, is also the thread where your UI event handling occurs, so to keep your application as responsive as possible, you should:

- Avoid any kind of long execution task, such as **input/output (I/O)** that could block the processing for an indefinite amount of time
- Avoid CPU-intensive tasks that could make this thread occupied for a long time

The following diagram displays the main interactions and components involved in the Looper line of execution thread:

![](img/35d01831c19688a44e50bce7c489007a_938_0.png)

The UI/Main thread, which has a Looper facility attached to it, holds a queue of messages (MessageQueue) with some unit of work to be executed sequentially.

When a message is ready to be processed on the queue, the Looper Thread pops the message from the queue and forwards it synchronously to the target handler specified on the message.

When the target Handler finishes its work with the current message, the Looper thread will be ready to process the next message available on the queue. Hence, if the Handler spent a noticeable amount of time processing the message, it will prevent Looper from processing other pending messages.

For example, when we write the code in an oncreate() method in the Activity class, it will be executed on the main thread. Likewise, when we attach listeners to user-interface components to handle taps and other user-input gestures, the listener callback executes on the main thread.

For apps that do little I/O or processing, such as applications that don't do complex math calculations, don't use the network to implement features, or don't use filesystem resources, this single thread model is fine. However, if we need to perform CPU-intensive calculations, read or write files from permanent storage, or talk to a web service, any further events that arrive while we're doing this work will be blocked until we're finished.

### Note

Since the Android 5.0 (Lollipop), a new important thread named **RenderThread** was introduced to keep the UI animations smooth even when the main thread is occupied doing stuff.

## The Application Not Responding (ANR) dialog

As you can imagine, if the main thread is busy with a heavy calculation or reading data from a network socket, it cannot immediately respond to user input such as a tap or swipe.

An application that doesn't respond quickly to user interaction will feel unresponsive—anything more than a couple of hundred milliseconds delay is noticeable. This is such a pernicious problem that the Android platform protects users from applications that do too much on the main thread.

### Note

If an app does not respond to user input within five seconds, the user will see the **Application Not Responding (ANR)** dialog and will be offered the option to quit the application.

The following screenshot shows a typical Android ANR dialog:

![](img/35d01831c19688a44e50bce7c489007a_940_0.png)

Android works hard to synchronize the user interface redraws with the hardware-refresh rate. This means that it aims to redraw at the rate of 60 frames per second—that's just 16.67 ms per frame. If we do work on the main thread that takes anywhere near 16 ms, we risk affecting the frame rate, resulting in jank—stuttering animations, jerky scrolling, and so on.
