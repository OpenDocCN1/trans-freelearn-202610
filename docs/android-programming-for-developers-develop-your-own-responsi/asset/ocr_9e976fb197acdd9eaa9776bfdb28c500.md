# # Threads

So, what is a thread? You can think of threads in Java programming in the same way you think of threads in a story. In one thread of a story, we might have the primary character battling the enemy on the frontline while in another thread, the soldier's family is getting by, day-to-day. Of course, a story doesn't have to have just two threads. We could introduce a third thread; perhaps the story also tells of the politicians and military commanders making decisions. And these decisions then subtly, or not so subtly, affect what happens in the other threads.

Programming threads are just like this. We create parts/threads in our program that control different aspects for us. Threads are especially useful when we need to ensure that a task does not interfere with the main (UI) thread of the app, or if we have a background task that takes a long time to complete and must not interrupt the main thread of execution. We introduce threads to represent these different aspects because of the following reasons:

+   - They make sense from an organizational point of view
+   - They are a proven way of structuring a program that works
+   - The nature of the system we are working on forces us to use them from time to time

In Android, we use threads for all three reasons simultaneously. It makes sense, it works, and we have to use the threads because of the design of the system.

Often, we use threads without knowing about it. This happens because we use classes that use threads on our behalf. SoundPool for example, loads sounds in a thread, and BaseAdapter sorts our data and loads up our ListView in a thread. Threads are also very useful in games, as we will see.

In gaming, think about a thread that is receiving the player's button taps for moving left and right and another thread that draws all the graphics to the screen.