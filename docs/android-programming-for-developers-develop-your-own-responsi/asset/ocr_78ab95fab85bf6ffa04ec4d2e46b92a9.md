The following diagram displays the main interactions and components involved
in the Looper line of execution thread:

![](bbox=[0.114, 0.142, 0.885, 0.517])

The UI/Main thread, which has a Looper facility attached to it, holds a queue of
messages (MessageQueue) with some unit of work to be executed sequentially.

When a message is ready to be processed on the queue, the Looper Thread
pops the message from the queue and forwards it synchronously to the target
handler specified on the message.

When the target Handler finishes its work with the current message, the Looper
thread will be ready to process the next message available on the queue. Hence,
if the Handler spent a noticeable amount of time processing the message, it will
prevent Looper from processing other pending messages.

For example, when we write the code in an oncreate() method in the Activity
class, it will be executed on the main thread. Likewise, when we attach listeners
to user-interface components to handle taps and other user-input gestures, the