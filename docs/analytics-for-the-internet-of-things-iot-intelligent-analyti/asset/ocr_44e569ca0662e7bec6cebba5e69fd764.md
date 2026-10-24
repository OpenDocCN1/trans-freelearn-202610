QoS 1 is used when delivery needs to be guaranteed and the use case can handle duplicate messages.

![](bbox=[0.11, 0.21, 0.9, 0.55])

# QoS 2

**Deliver exactly once** - Make sure they get it, but make sure they only get it once - they get confused.

QoS 2 guarantees the message is delivered and delivered only once. As can be seen in the diagram, there are multiple back and forth communications to orchestrate this guarantee. The state of the communication needs to be stored by both the sides until full confirmation of delivery is communicated back to the publisher. This requires both a higher level of power on the device and a larger memory footprint in order to store the more complex state. It also takes a bit longer to complete QoS 2 communications.