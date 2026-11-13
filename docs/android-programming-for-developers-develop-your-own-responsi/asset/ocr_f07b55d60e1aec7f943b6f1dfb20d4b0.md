# Dalvik runtime

The Dalvik VM (DVM) runtime, created by Dan Borstein, was the first runtime for the platform and is a register-based virtual machine that was created to run the Java code efficiently in a constrained runtime with a limited amount of power processing, RAM, and electric power.

Dalvik’s creators claim that the DVM is, on an average, around 30% more efficient than the standard Java VM (Oracle). According to Bornstein, it requires 30% less instructions and 35 % less coding units.

Clearly, Google has gone to great lengths to squeeze every drop of performance out of each mobile device to help developers build responsive applications.

The virtual machine, which runs the Java code compiled and transformed to the dex format over the dx tool, runs on a Linux process with its own memory space and file descriptors. It also manages its own group of threads.

In more advanced architectures, an Android application might run a service in a separate process and communicate over the IPC mechanism, but most of the time, it runs on a single self-contained process.

The dex file and application resources are packed in an Android application package (APK) by the AAPT and installed over Google Play in the end user devices.

# Note

The application store distribution model has become extremely popular on the mobile platforms since the launch of the Apple iPhone in 2007.

Since Android 2.2 the DVM comes with a trace-based Just-In-Time (JIT) compilation feature that actively optimizes every time the application runs some short segments of frequently used bytecode called traces.

The generated machine code provides significant performance improvements in the application execution and on the time spent on some intensive CPU tasks, and thereafter, decreases the battery power used.