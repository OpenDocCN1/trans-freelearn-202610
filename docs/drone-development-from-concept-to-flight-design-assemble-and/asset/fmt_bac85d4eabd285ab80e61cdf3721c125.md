
# Mounting long-range telemetry components

You’ll need two telemetry modules – one for your drone and one for your ground station. These modules are typically designed to be compact and lightweight for ease of mounting.

Each telemetry module requires an antenna. Mount the antennas in locations that provide a clear line of sight to each other. Higher placement and less obstruction offer better signal quality.

You must securely mount the telemetry modules and antennas on your drone and ground station using suitable mounting hardware, such as brackets, clamps, or adhesive mounts.

## Selecting a protocol

Long-range telemetry systems often use protocols such as `Long Range Systems` (`LRS`), MAVLink, or proprietary protocols specific to the manufacturer. Ensure compatibility between your modules and choose a protocol that suits your requirements. Generally, within the scope of this chapter, we are considering the serial protocol with RFD900 telemetry modules, which we studied earlier.

## Connection and configuration

Here is how to connect and configure a long-range telemetry system with your drone, specifically using an LRS system as an example:

1. **Hardware connection**: Connect one telemetry module to your drone’s flight controller. This typically involves connecting the telemetry module’s TX and RX wires to a free UART port on the flight controller. Ensure that the voltage levels are compatible (usually 3.3V or 5V).

   Then, connect the other telemetry module to your ground station’s computer or telemetry device via USB or a compatible serial port.

2. **Configuring the ground station**: Install the ground station software or telemetry configuration software provided by the manufacturer on your computer. For RFD tools, there is software named `RFD tools` that you can use to configure and bind the telemetry radios.

   Then, connect the ground station telemetry module to your computer. Once you’ve done this, you must configure the telemetry module’s settings using the manufacturer’s software. This may include selecting the appropriate protocol, configuring the air and ground unit IDs, and adjusting transmission power settings.

   Now we will configure the drone (flight controller).

3. Power on your drone and connect it to the ground station via USB or telemetry.

4. Open the configuration software for your drone’s flight controller (for example, Betaflight, ArduPilot, and so on).

5. Configure the telemetry protocol and port so that they match your telemetry module’s settings. Ensure that the baud rate and other settings match between the flight controller and telemetry module.

6. Enable telemetry or MAVLink data transmission on the flight controller. This step is not necessarily required since some flight controllers are pre-configured. This typically involves enabling a telemetry port and setting its parameters.

7. Write the configuration changes to the flight controller.

   We will then do testing and verification.

8. Power up your drone and ground station.

9. Check that telemetry data is being transmitted from the drone to the ground station and vice versa.

10. Monitor telemetry data on your ground station software, such as GPS position, altitude, battery voltage, and other relevant information.

By following these steps and using the appropriate long-range telemetry components and protocols, you can successfully configure a reliable long-range telemetry communication link with your drone. Remember to consult the user manuals and documentation provided by the telemetry system manufacturer for specific setup and configuration details.

# Setting up and configuring avionics

The avionics configuration of a drone refers to the suite of electronic systems and components that are responsible for its navigation, control, and communication. This critical aspect of drone design typically includes GPS receivers for accurate positioning, flight controllers for stabilization and control, telemetry systems for data transmission, sensors such as accelerometers and gyroscopes for flight data acquisition, and often advanced features such as obstacle avoidance sensors and cameras for autonomous navigation and data capture. Avionics play a vital role in ensuring safe and precise flight operations, making them an integral part of modern drone technology.

## Firmware flashing

Flashing firmware to a flight controller using `Aero Ground Control Station` (`AeroGCS`) for a quadcopter is a crucial process to ensure that your flight controller has the latest software and features. Here’s a general guide on how to flash firmware using AeroGCS:

1. **Prepare your equipment**: Ensure that your quadcopter is powered off and disconnected from the AeroGCS-equipped device (for example, your laptop or tablet).

2. **Launch AeroGCS**: Open AeroGCS on your device.

3. **Connect the flight controller**: Connect your flight controller to your device using the appropriate communication method (USB, telemetry radio, and so on). Ensure that the connection is stable.

4. **Access the firmware update menu**: In AeroGCS, navigate to the firmware update or flash firmware menu. The location of this menu may vary, depending on the AeroGCS version you’re using.

5. **Select the firmware**: Select the firmware file for your flight controller. Select the file to upload it to AeroGCS.

6. **Flash firmware**: Follow the onscreen instructions to initiate the firmware flashing process. Typically, this involves clicking on a `Flash` or `Update` button.

7. **Verify the firmware’s version**: Once the firmware update is successful, verify that the flight controller now has the updated firmware version. This information is usually displayed within AeroGCS.

8. **Test and verify**: After the firmware update, conduct a ground test to ensure that the flight controller is operating correctly. Verify that all sensors and functions are working as expected.

## Configuring the flight controller

Configuring a flight controller for a quadcopter using AeroGCS typically involves several key parameters and settings to ensure safe and precise flight operations. Here’s a list of common configurations that are usually required and a general guide on how to do them using AeroGCS:

### `Motor layout and propeller configuration`

**Purpose**: Specify the motor layout and propeller type so that they match your quadcopter’s physical configuration.

**How to do it**:

1. Connect your quadcopter to AeroGCS.
2. Access the motor layout and propeller configuration settings.
3. Select the appropriate motor layout (for example, X-configuration, H-configuration, and so on) and enter details about your quadcopter’s propellers, including diameter and pitch.

### `Flight mode configuration`

**Purpose**: Define different flight modes (for example, stabilize, altitude hold, GPS position hold, and `return-to-home` (`RTH`)) and assign them to transmitter switches or control channels.

**How to do it**:

1. Access the flight modes configuration menu.
2. Assign flight modes to specific transmitter switches or control channels.
3. Configure the parameters for each flight mode, if necessary.

### `RC transmitter calibration`

**Purpose**: Ensure that your quadcopter responds accurately to transmitter inputs.

**How to do it**:

1. Connect your drone to AeroGCS.
2. Access the RC transmitter calibration menu.
3. Calibrate the transmitter endpoints and verify that all control channels operate correctly.

### `Safety and failsafe configuration`

**Purpose**: Establish failsafe mechanisms to ensure safe operation in case of signal loss or critical errors.

**How to do it**:

1. Access the safety and failsafe configuration menu.
2. Define actions to be taken during signal loss, low battery, or other emergencies (for example, RHT or auto-land).

### `Sensor calibration`
