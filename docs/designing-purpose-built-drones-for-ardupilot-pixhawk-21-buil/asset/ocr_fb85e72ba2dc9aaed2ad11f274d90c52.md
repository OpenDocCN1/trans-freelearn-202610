# Wizard

The following screenshot shows the interface that pops up when clicking on the Wizard tab. This is a step-by-step configuration wizard to help get the basic setup of your Pixhawk in place:

![](bbox=[0.244, 0.227, 0.756, 0.514])

# Mandatory hardware

This section allows you to calibrate/setup all of the peripherals that are required to make Pixhawk drive your vehicle properly. Let's take a look at each:

+   - **Accel Calibration**: This allows to calibrate the accelerometers. You'll want to do this if you are getting a Bad AHRS error in the Flight Data screen. Simply enter this menu and click on Calibrate Accelerometer to open the calibration wizard. The Calibrate Level button lets you calibrate the correct level position of Pixhawk. This is essential for the aircraft, otherwise they may pitch/roll when Pixhawk believes it is flying level. In multicopters, this can cause movement when trying to hover if not calibrated properly. In fixed-wings, this can cause oscillation on the pitch/roll axis if not calibrated properly. If you alter the Pixhawk location on your aircraft, this is an essential process as well.