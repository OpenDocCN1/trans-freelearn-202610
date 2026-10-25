We should dig deeper into instrumentation only after a benchmarking test indicates that further analysis is required. It is also very important to benchmark by simulating actual platform behavior as much as possible if we want a realistic data sample. As such, we should never accept benchmarking data that was generated through Editor Mode as representative of real gameplay, since Editor Mode comes with some additional overhead costs that might mislead us, or hide potential race conditions in a real application. Instead, we should hook the profiling tool into the application while it is running in a standalone format on the target hardware.

Many Unity developers are surprised to find that the Editor sometimes calculates the results of operations much faster than a standalone application does. This is particularly common when dealing with serialized data like audio files, Prefabs and Scriptable Objects. This is because the Editor will cache previously imported data and is able to access it much faster than a real application would.

Let’s cover how to access the Unity Profiler and connect it to the target device so that we can start to make accurate benchmarking tests.

Users who are already familiar with connecting the Unity Profiler to their applications can skip to the section titled _The Profiler window_.

# # Launching the Profiler

We will begin with a brief tutorial on how to connect our game to the Unity Profiler within a variety of contexts:

+   - Local instances of the application, either through the Editor or a standalone instance
+   - Local instances of a WebGL application running in a browser
+   - Remote instances of the application on an iOS device (for example, iPhone or iPad)
+   - Remote instances of the application on an Android device (for example, an Android tablet or phone)
+   - Profiling the Editor itself