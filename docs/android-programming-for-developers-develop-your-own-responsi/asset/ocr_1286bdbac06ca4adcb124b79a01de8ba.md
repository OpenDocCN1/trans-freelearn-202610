# Problems with threads

Programs with multiple threads can have problems, like the threads of a story in which if proper synchronization does not occur, then things go wrong. What if our soldier went into battle before the battle or war even existed? Weird.

What if we have a variable, int x, that represents a key piece of data that three threads of our program use. What happens if one thread gets slightly ahead of itself and makes the data "wrong" for the other two? This problem is the problem of correctness caused by multiple threads racing to completion obliviously— because after all, they are just dumb code.

The problem of correctness can be solved by close oversight of the threads and locking, locking meaning temporarily preventing execution in one thread to be sure things are working in a synchronized manner. This is kind of like preventing a soldier from boarding a ship to war until the ship has actually docked and the gang plank has been lowered, avoiding an embarrassing splash.

The other problem with programs with multiple threads is the problem of deadlock, where one or more threads become locked waiting for the right moment to access int x, but that moment never comes, and eventually the entire program grinds to a halt.

You might have noticed that it was the solution to the first problem (correctness) that is the cause of the second problem (deadlock). Now, consider all we have just been discussing and mix it in with the Android activity lifecycle, and it’s possible you start to feel a little nauseous with the complexity.

Fortunately, the problem has been solved for us. Just as we use the Activity class and override its methods to interact with the lifecycle, we can also use other classes to create and manage our threads. Just as with Activity, we only need to know how to use them not how they work.

"So, why tell me all this stuff about threads when I didn’t need to know?" you rightly ask. Simply because we will be writing code that looks different and is structured in an unfamiliar manner, if we can:

+   Accept that the new concepts we introduce are what we need to work with