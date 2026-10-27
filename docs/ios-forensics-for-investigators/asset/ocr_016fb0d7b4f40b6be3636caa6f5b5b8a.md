that the latter does not perform well inside buildings; it is also slower than other technologies and quickly drains a device's battery. A-GPS works by augmenting satellite GPS by using external sources (such as cell towers) that enhance the speed, quality, and precision of satellite signals. Data received by external sources is then consolidated to figure out the device's precise location.

The following is a list of sources that power A-GPS:

+   - Satellite GPS
+   - Cell towers
+   - Wi-Fi networks
+   - Bluetooth devices

We will look into these in more detail in the following sections.

You may be wondering how an iOS device manages to resolve locations from Wi-Fi networks or cell towers; this is implemented by Apple using a **peer-based mechanism** where each iOS device contributes to create a map of networks and devices.

In general, this mechanism consists of three steps:

+   1. If Location Services is enabled, each iOS device periodically collects and stores the current GPS position and a list of surrounding cell towers and Wi-Fi hotspots.
+   2. This data is then sent to Apple servers anonymously, where it is consolidated with other collected information and third-party providers.
+   3. The data is synced back to the iOS device when a location service requests the device's position.

For instance, if a location service requests the device's position and satellite GPS is unavailable, iOS will scan for nearby cell towers and query Apple's peer-generated map to find any geolocation coordinates associated with