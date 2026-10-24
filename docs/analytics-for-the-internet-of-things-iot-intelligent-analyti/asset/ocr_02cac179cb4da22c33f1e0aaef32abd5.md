There are several types of thing:

+   - **Things:** This represents a real-world asset, device, or system.
+   - **Remote thing:** A remote thing is a special type of thing that represents an asset in a remote location. When using a ThingWorx Edge SDK, the edge devices where your application is running needs to be created in ThingWorx Composer using the RemoteThing template.
+   - **Custom things:** Extensions provide custom thing templates that you can use to create the custom things to represent your devices.

# Properties

A property is a variable that represents a behavior of a thing.

Properties have either of the following:

+   - **Remote bindings:** Remote bindings support egress. They can be written to the edge device when it connects to ThingWorx. The edge device must know of any Properties with Remote bindings.
+   - **Non-remote bindings:** These are not sent to the edge device when written and are not read from the edge device when a value is requested.

# Services

A service represents a function that a thing can perform. It can be defined as part of a thing, a thing template, or a thing shape.

# Events

An event is triggered from a change in the state or value of a property. It sends data to an object that subscribes to it.