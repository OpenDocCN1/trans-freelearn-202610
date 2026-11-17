
# Connecting the Drone to GCS

In the following screenshot, we can see a popup; the software requests to connect a device to it. Once you have clicked on `Yes`, the next steps to connect the drone to the GCS via a radio module will appear. This will ask you to select the protocols that we want to connect the drone to and various other parameter configuration options:

![Figure 7.13 – Prompt to connect to a drone](image/Image96562.jpg)

Figure 7.13 – Prompt to connect to a drone

The following are the steps for establishing the connection for any of these methods:

1.  Choose a communication link. It could be `Serial`, `TCP`, or `UDP`. Select the suitable link based on the communication device you choose, as shown in the following screenshot. Since we have mentioned the use of RFD 900, we will go ahead with choosing the `Serial` connection:

![Figure 7.14 – Connection settings](image/B19566_07_14.jpg)

Figure 7.14 – Connection settings

Since we have chosen the serial connection, we will go ahead and look at the settings in relation to this option.

1.  The `Serial Port` setting is required when we are connecting the drone using a serial protocol, such as UART, and serial radio frequency modules, such as RFD 900 433Mhz, via USB. Since these devices work on serial protocols, `Serial` has to be selected at the top:

![Figure 7.15 – Serial Port Setting](image/B19566_07_15.jpg)

Figure 7.15 – Serial Port Setting

There are some parameters that need to be set before proceeding, which are as follows:

*   **`Serial Port`**: When we connect the RFD 900 or any other module via USB, it establishes its own communication port through which the module can be accessed. This communication port will appear on the drop-down menu of `Serial Port`.
*   **`Baud rate`**: We know that in serial communication, data is communicated via the transmission of bits (digital data) from one device to another through transmission media. The baud rate is the rate at which the number of signal elements or changes to the signal occurs per second when it passes through a transmission medium. The higher the baud rate, the faster the data is sent/received. For a wired communication via a USB cable, the baud rate is mostly 115,200, and for wireless connections, it is 57,600 if we are connecting to the flight controller.
*   **`Data bits`**: Any messages that are transmitted over a serial port area sent via data bits. These can be a command, a sensor reading, an error message, and more. The majority of serial ports use 5 to 8 data bits for transmission, and text data is transmitted using 7 or 8 bits. These bits are used so that characters in the ASCII format can be used to represent the data.
*   **`Parity`**: Parity is an error-checking procedure that uses the number of ones included in the transmitted bits. The number of ones must always be the same, either odd or even, for each group of bits that are transmitted without error.

Click on `Connect` on the top right of the window, and the drone should connect to the GCS.

1.  **Connection via TCP and UDP**: If we are using a smart radio, such as Doodle Labs or Microhard, which we looked at in previous chapters, we can use a TCP/UDP connection to connect to a network. In the TCP connection, the user can set the host address, listing port, target address, and target port of the host module, and the drone will be connected via TCP. Hence, for `COMM LINK SETTING`, shown in the following screenshot, `TCP` now has to be selected:

![Figure 7.16 – TCP connection in AeroGCS](image/B19566_07_16.jpg)

Figure 7.16 – TCP connection in AeroGCS

`UDP` stands for `User Datagram Protocol`, which is a transport layer protocol in the OSI model. It is not very reliable for data communication because in UDP, each packet is sent independently, without guarantees of delivery or orders; however, TCP provides a reliable connection to the system as it guarantees that data sent from one end is received correctly by the other end. In AeroGCS, a TCP connection is preferred over a UDP connection.

![Figure 7.17 – UDP connection in AeroGCS](image/B19566_07_17.jpg)

Figure 7.17 – UDP connection in AeroGCS

Hence, in the previous examples, we have seen the different methods of connecting to a drone (flight controller) through different protocols, which have to be selected accordingly. This will help us to establish a connection between the GCS and the drone. We can now command and configure the drone.

# RPA or Drone Configuration

When we connect a drone to the GCS, it’s mandatory to set up the necessary configurations of the drone and do the required calibration before any flights. The configuration of the drone is required so that the flight controller knows the exact values of the sensors, the airframe type, the battery status, the communication strength, and so on, which are required during the flight. The flight controller makes necessary decisions based on this data.

Checking sensor calibration, the motors, the ESCs, and the compass before a flight is important; this should be done before any flight.

The camera parameters should be set to capture the maximum number of best-quality images and videos of the area over which the drone is flying.

Necessary failsafe actions are to be taken for the emergency recovery of the drone in the case of a battery failure or a loss of communication.

The parameters related to the battery, power, and others are adjusted and set very easily with the RPA configuration.

Some important parameters to be set are as follows:

*   **Selection of airframe**: Airframe selection is necessary, as it lets the flight controller know the type of drone (we have built a quadcopter, hexacopter, and octacopter, which are defined under the frame class).

    Next, the configuration of quad/hexa/octa and other similar drones needs to specify whether an `X shape`, `+ shape`, or `V shape` is being used. These options are given in the drop-down menu.

    It’s mandatory to reboot the flight controller once we have selected the airframe for the changes to take effect. Due to this selection, the `ESC` gets activated, and the basic physics of the drone is calculated by the flight controller for a stable flight.

*   **Safety parameters**: Safety parameter settings are important to recover the drone in case of an emergency. These are some of the failsafe measures that are preloaded into the drone in case of a failsafe situation.

*   **Failsafe**: Failsafe means if any of the crucial components of the drone stop working or malfunction, then what action must be taken by the drone itself for the safe recovery of the drone? These failsafe actions can be selected in this menu before the flight.

We will study the basic and important failsafes here that need to be set up before the flight:

*   **Battery failsafe**: It triggers when the battery percentage of the drone falls to an already-set percentage, for example, 20%. We can choose an action for what the drone needs to do in that case.

![Figure 7.18 – Different battery failsafes](image/B19566_07_18.jpg)

Figure 7.18 – Different battery failsafes

The following are some of the battery failsafes and their actions:

*   **`Low Action`**: `None`, `LAND`, `RTL`, `SmartRTL`, `SmartRTL or Land`, `Terminate`
*   **`Critical Action`**: `None`, `LAND`, `RTL`, `SmartRTL`, `SmartRTL or Land`, `Terminate`
*   **`Low Voltage Threshold`**: Battery voltage that triggers the low action
*   **`Critical Voltage Threshold`**: Battery voltage that triggers the critical action
*   **`Low mAh Threshold`**: Battery capacity that triggers `Low Action`
*   **`Critical mAh Threshold`**: Battery capacity that triggers `Critical Action`

We can even choose to activate or disable the failsafe action based on the needs of the applications.
