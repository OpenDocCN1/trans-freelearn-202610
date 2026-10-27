
## Recommended VTOL RTL procedure

There are several ways to handle **RTL** with a hybrid like this, but here's what I recommend:

- 1. Head back (RTL) using the airplane **RTL** mode. This gets you to the vicinity quickly.
- 2. Switch to **QRTL** when close to the home point. If `Q_RTL_ALT`, `RTL_RADIUS`, and `Q_RTL_MODE` are set, this happens automatically.
- 3. Switch to **QLAND** and again, if `Q_RTL_ALT`, `RTL_RADIUS`, and `Q_RTL_MODE` are set, this will happen automatically.

I find this to be the most efficient procedure. No wasting battery time on circling, or other maneuvers. Just fly in, switch modes, and land.

## Notes before attempting any transitions

I know, after all that effort, you're going to be itching to see it transition. It's a long and painful road to tune your multicopter mode before even thinking of transitioning.

Make sure you adjust all of the Q tuning parameters so that the multicopter mode is flying as stable as possible before doing any hybrid flights. You'll save yourself a lot of expense and headache.

When attempting transitions, make sure you are well above a minimum height of 20 m **above ground level (AGL)**. Until you get all of your Q transition parameters set, you are very likely to lose altitude, or even lose stability on your first tests. Even then, stay above 20 m AGL.

## The Flight Data interface

The following image shows the flight data interface for Mission Planner:

![](img/504adb30bdfce87f38258e08f11180af_241_0.png)

The top-left window represents the Heads-up Display (HUD)—where you can instantly tell the orientation, altitude, and status of the aircraft. Warning messages appear as red text (such as the DISARMED and Bad AHRS warnings we see in the image).

The large window is a projected satellite map showing the location of the aircraft/multicopter/rover (in this case, a fixed wing aircraft). Protruding from the aircraft is a purple line representing the direction the aircraft is pointing at.

The window on the lower-left shows current data with several tabs to see data presented in different formats, and even transmits signals to the Pixhawk. The tabs are follows:

- **Quick:** These are the numerical values of basic telemetry data from the vehicle.
- **Actions:** This allows the user to send commands to Pixhawk such as Loiter, Arm, and so on.
- **PreFlight:** This shows a checklist and status for preflight before launching the Pixhawk vehicle. Green represents the items that are ready and red represents items which may require attention.
- **Gauges**: This shows an analog view with similar information to the quick screen. This analog view uses graphic representation in the form of virtual gauges rather than simple numbers.
- **Status**: This provides a detailed readout of every single telemetry parameter the vehicle is experiencing.
- **Telemetry Logs**: This lets the user load flight logs from Pixhawk into the computer and replay the mission to evaluate the mission post-mortem.
- **DataFlash Logs**: This is used to troubleshoot problems with Pixhawk. If you are experiencing problems with tuning or missions, these logs can be extremely useful to spot errors. Also, when asking for help from a community, the first question they’ll ask is for you to provide these logs for analysis.
- **Messages**: This provides a real-time view of log entries as they happen.

## The Flight Plan Screen

The following image shows the Flight Plan Screen (without fixed-wing tuning mission loaded):

![](img/504adb30bdfce87f38258e08f11180af_242_0.png)

The main window shows a satellite map of the mission area. The yellow lines represent the desired path of the vehicle, while the white circles represent the area for the turning arc (turning radius). The green markers represent the actual waypoint targets for the mission.

The bottom window is a chart representing all of the parameters for the mission. In this window, you can add events (such as Do Jump, which sends the vehicle back to a specific waypoint). If your camera is attached to Pixhawk, you can tell the camera to take an image/start recording. Flight modes can be set to change as well (such as "Land"). Any command Pixhawk is capable of executing can be triggered using this interface.

The window on the far right allows you to save missions for later use (to your computer), load previous missions, change the type of satellite map. Different maps can prove to be more/less detailed, and can also prove to be more/less current. Finally, at the bottom of this window, is a section that allows you to read the current waypoints loaded into Pixhawk, or write the waypoints you've just created. You must write waypoints for Pixhawk to execute the mission. The home point is also displayed here.

## The Initial Setup screen

The following screenshot shows the initial setup screen's Install Firmware tab. This screen allows you to repurpose a Pixhawk module for other applications (such as Rover):

![](img/504adb30bdfce87f38258e08f11180af_243_0.png)

## Wizard

The following screenshot shows the interface that pops up when clicking on the Wizard tab. This is a step-by-step configuration wizard to help get the basic setup of your Pixhawk in place:

![](img/504adb30bdfce87f38258e08f11180af_244_0.png)

## Mandatory hardware

This section allows you to calibrate/setup all of the peripherals that are required to make Pixhawk drive your vehicle properly. Let's take a look at each:

- **Accel Calibration**: This allows to calibrate the accelerometers. You'll want to do this if you are getting a Bad AHRS error in the Flight Data screen. Simply enter this menu and click on Calibrate Accelerometer to open the calibration wizard. The Calibrate Level button lets you calibrate the correct level position of Pixhawk. This is essential for the aircraft, otherwise they may pitch/roll when Pixhawk believes it is flying level. In multicopters, this can cause movement when trying to hover if not calibrated properly. In fixed-wings, this can cause oscillation on the pitch/roll axis if not calibrated properly. If you alter the Pixhawk location on your aircraft, this is an essential process as well.
- **Bluetooth Setup**: An area to set up the Bluetooth module (like the one we used for the golf trolley setup). Because of its limited range, Bluetooth should not be used for aircraft or fast-moving vehicles. Keep in mind that Bluetooth has a range of about 30 metre.
- **Parachute**: This required an area to set up a parachute deployment system. These systems are not widely used, but can prove to be useful in the event of a failure of an aircraft (especially over people), or even in place of landing a vehicle. It should be noted that (especially on multicopters), the aircraft can become tangled in the parachute lines and parachutes should be viewed as the last chance, not a reliability.
- **ESP8266**: This is the area where you would setup the ESP8266 Wi-Fi module. As we discussed in our trolley chapter, this is another way of controlling/getting telemetry from your drone. Wi-Fi does have a limited range (about 100 m), but can prove to be useful (especially with Apple iOS devices which are Bluetooth prohibitive).
- **Antenna Tracker**: Longer range drones, which require a special **Beyond Visual Line of Sight (BLOS)** permit in the US benefit greatly from using a directional antenna, which constantly points toward a target. This is especially useful with high-quality video systems for live coverage without interference. This area is where you can set up and tune your antenna tracker.

## Config/Tuning screen

As the name implies, this area is where the Pixhawk administrator would configure and tune the aircraft. Since some of these menus are only available with an advanced layout, first we'll cover the Planner section (which will give you access to all the advanced features).

## Planner

The following screenshot shows the options for the **Planner** tab. This is the area where we configure the Mission Planner software itself:
