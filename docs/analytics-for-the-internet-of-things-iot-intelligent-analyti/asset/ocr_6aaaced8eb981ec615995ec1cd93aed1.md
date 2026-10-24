Azure IoT Hub has some key features, which give some insight into how it works:

+   - **Authentication and connectivity security:** Each device is provisioned with its own security key that allows it to connect to IoT Hub. Identities and keys are stored in the IoT Hub identity registry.
+   - **Device twins:** These are JSON documents that store state information such as configuration and parameter values. It is stored in the cloud and persists for each device connected to the IoT Hub. A twin allows you to store, synchronize, and query device data even if the device is offline.
+   - **Connectivity monitoring:** Here, you can receive detailed logs on identity management and connectivity events for your devices.
+   - **The IoT protocols support:** MQTT, HTTP, and AMQP protocols are supported by Azure IoT device SDKs that you would install on your IoT device. They are also supported through an exposed public protocol in the event you cannot use the SDK on your device.
+   - **IoT protocol extensibility:** You can support custom protocols by either of the following:
+     - **Creating a field gateway:** You can use the Azure IoT gateway SDK to convert your custom protocol to one of the three protocols understood by IoT Hub.
+     - **Customizing the Azure IoT protocol gateway:** This runs in the Azure cloud and is an open source component.
+   - **Scale:** Millions of devices can be connected at the same time with millions of events per second flowing through the hub.

# # Applying big data technology to storage

With IoT data flooding into your cloud environment and after processing and transforming it, the next problem to solve is how to store it. The solution should support holding large datasets and be easy to interact with for analytics.

# ## Hadoop

Hadoop is an open source effort that falls under the umbrella of the Apache Software Foundation. As defined by the official project documentation, *The Apache Hadoop project develops open source software for reliable, scalable, distributed computing.* Hadoop is available for free in its pure open source form.