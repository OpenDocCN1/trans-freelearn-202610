![](bbox=[0.231, 0.104, 0.768, 0.337])

Note that a `MessagingSystem` Singleton object will be created during Scene initialization,
when either the `EnemyManagerWithMessagesComponent` object's or
`EnemyCreatedListenerComponent` object's `Start()` methods are called (whichever
happens first), since that is when they register their delegates with the Messaging System,
which accesses the `Instance` property, and hence creates the necessary `GameObject`
containing the Singleton Component. No additional effort is required on our part to create
the `MessagingSystem` object.

# ## Message cleanup

Since message objects are classes, they will be created dynamically in memory and will be
disposed of shortly afterward when the message has been processed and distributed among
all listeners. However, as you will learn in Chapter 8, Masterful Memory Management, this
will eventually result in a garbage collection, as memory accumulates over time. If our
application runs for long enough, it will eventually result in the occasional garbage
collection, which is the most common cause of unexpected and sudden CPU performance
spikes in Unity applications. Therefore, it is wise to use the Messaging System sparingly
and avoid spamming messages too frequently on every update.

The more important clean-up operation to consider is deregistration of delegates if an object
needs to be destroyed. If we don't handle this properly, then the Messaging System will
hang on to delegate references that prevent objects from being fully destroyed and freed
from memory.