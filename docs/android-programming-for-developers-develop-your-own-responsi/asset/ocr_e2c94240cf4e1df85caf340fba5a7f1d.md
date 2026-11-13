# Understanding Looper

Before we can understand Looper, we need to understand where it gets its name from.

# Note

A loop is a group of instructions that are repeated continually until a termination condition is met.

Following this definition, Android's Looper executes on a thread that has a MessageQueue, executes a continuous loop waiting for work, and blocks when there is no work pending. When work is submitted to its queue, it dispatches it to the target Handler defined explicitly on the Message object.

# Note

A message is a notification object containing a description and arbitrary data object that can be sent to a Handler.

The Looper on Android is an implementation of a common UI programming concept known as an event loop. Later, at the end of this processing sequence, the Handler will process the Message and execute your domain logic in order to solve an application user problem.

The Looper sequence on Android follows these steps:

+   1. Wait until a Message is retrieved from its MessageQueue
+   2. If logging is enabled, print dispatch information
+   3. Dispatch the message to the target Handler
+   4. Recycle the Message
+   5. Go to step 1

As mentioned on the previous chapter, the main thread implicitly creates its own Looper to sequentially process all that is needed to keep the application running and to manage the interaction between the application components.

To access the main thread's Looper you want access to the main thread's Looper