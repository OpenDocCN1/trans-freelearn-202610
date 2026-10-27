# Testing and driving

Before we take it out to the field, we'll want to test the connections. Close the Mission Planner software, remove the USB cable, and attach the USB telemetry dongle to your computer. With the wheels off the ground, power on your transmitter and rover by plugging the battery in, but do not arm the rover.

After starting the Mission Planner software, you may be asked again to find the rover's com port. In your device manager, the telemetry unit should be visible under the Ports tab, this time as a Silicon Labs device.

Upon successfully finding the telemetry dongle, you'll see this screen inside Mission Planner:

![](bbox=[0.104, 0.376, 0.896, 0.716])

After clicking on the Connect button (top right of the interface), you should see your rover at your location. Yep... that's my house.

Let's do some initial tuning and show you how it's done. Before we actually drive the rover, we need to make sure it's not just going to speed away at 70+ mph into oblivion.