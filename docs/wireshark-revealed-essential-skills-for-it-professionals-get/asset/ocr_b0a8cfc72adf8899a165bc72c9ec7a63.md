# How it works...

The Time-Sequence Graph (Stevens) is a simple graph that counts the TCP sequence numbers over time. Since TCP sequence numbers count the bytes sent by TCP, these are actually application bytes (including application headers) sent from one side to another.

This graph (as we will learn in the TCP and applications chapters) can give us a good indication of the application's behavior. For example, a diagonal line means a good file transfer, while a diagonal line with interrupts shows a problem in transfer. A diagonal line with a high gradient indicates fast data transfer, while a low gradient indicates a low rate of transfer (depends on the scale of course).