# ThingWorx Core

ThingWorx Core is a software platform environment that allows you to design, run, and implement analytics for IoT applications that control and report data from remote devices. These devices could be sensors, consumer electronics, or industrial equipment.

ThingWorx uses a representational object-based design. This means that you create software objects to represent your IoT devices and other assets. The representation includes relevant properties and related data items.

You then use the objects to create applications, which can monitor and manage your IoT devices. You can create dashboards, implement response logic, and integrate third-party applications.

The ThingWorx Core is the hub of your ThingWorx environment. You logically define behavior and relationships between IoT devices or remote assets that are set up in your environment. Once the actual devices have been modeled in the software, they can register and communicate with the Core. You can then collect data and manage the physical devices.

ThingWorx Core includes two main tools for you to create IoT solutions:

+   - **ThingWorx Composer**: This is a modeling environment where you set up the remote assets, business logic, data storage, and security.
+   - **ThingWorx Mashup Builder**: This is a drag and drop tool where you can quickly create dashboards and mobile interfaces without needing to write code. This is where you can do things like show a location on a map and chart sensor value trends.

# ThingWorx Connection Services

ThingWorx Connection Services handle communication and connectivity between the Core and remote assets. Components can handle connectivity over different protocols and with different device clouds. They handle message routing to and from the remote devices and also message translation when required.