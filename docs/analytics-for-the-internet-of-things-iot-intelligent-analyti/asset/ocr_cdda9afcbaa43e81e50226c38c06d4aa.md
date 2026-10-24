# What is a message queue?

![](bbox=[0.105, 0.145, 0.894, 0.352])

New data comes into a queue as a message, it goes into line for delivery, and then it is delivered to the end server when it gets its turn. The process adding a message is called the **publisher**, and the process receiving the message is called the **subscriber**.

The message queue exists regardless of whether the publisher or subscriber is connected and online. This makes it robust against intermittent connections (intentional or unintentional). The subscriber does not have to wait until the publisher is willing to chat and vice versa.

The size of the queue can also grow and shrink as needed. If the subscriber gets behind, the queue just grows to compensate until it can catch up. This can be useful if there is a sudden burst in messages by the publisher. The queue will act as a buffer and expand to capture the messages while the subscriber is working through the sudden influx.

There is a limit, of course. If the queue reaches some set threshold, it will reject (and you will most likely lose) any incoming messages until the queue gets back under control.

Here is a contrived but real-world example of how this can happen:

> Joe Cut-rate (the developer): *Hey, when do you want this doo-hickey device to wake up and report?*

> Jim Unawares (the engineer): *Every 4 hours*

> Joe Cut-rate: *No sweat. I’ll program it to start at 12 a.m. UTC, then every 4 hours after. How many of these you gonna sell again?*