# # Memory sharing and Zygote

Basically, the platform runs an instance of DVM/ART for each application, but large optimization of the platform is brought about by the way a new DVM instance is created and managed.

A special process called the Zygote (first life cell in an animal's reproduction)—the process that all the Android applications are based on—is launched when an Android device initially boots.

The Zygote starts up a virtual machine, preloads the core libraries, and initializes various shared structures. It then waits for instructions by listening on a socket.

When a new Android application is launched, the Zygote receives a command to create a virtual machine to run the application on. It does this by forking its pre-warmed VM process and creating a new child process that shares some memory portions with the parent, using a technique called copy-on-write (COW).

The COW technique, available on most Unix systems, only allocates new memory on the child process when the process tries to change the memory cloned from the parent process.

This technique has some fantastic benefits, as listed in the following:

+   - First, the virtual machine and core libraries are already loaded into the memory. Not having to read this significant chunk of data from the filesystem to initialize the virtual machine drastically reduces the startup overhead.
+   - Second, the memory in which these core libraries and common structures reside is shared by the Zygote with all other applications, resulting in saving a lot of memory when the user is running multiple apps.