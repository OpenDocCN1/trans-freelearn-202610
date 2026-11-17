
## GNSS Device Examples

- **Drotek DP0601**: Drotek DP0601 is a **real-time kinematics** (**RTK**)-based GNSS device that supports GPS, GLONASS, Beidou, Galileo, and QZSS signal reception, gives centimeter-level precision, and comes with built-in support for standard RTCM corrections. It has built-in communication protocols that support both `UART` serial protocols and `SPI`, making it suitable for use with multiple devices.

- **Synerex MDU-2000**: MDU-2000 is an RTK-based GNSS receiver that comes with a dual antenna for centimeter-level accuracy. The device does not get impacted by external electromagnetic fields due to its dual antenna technology that provides its true heading even in high magnetic field interference.

Hence, we got an idea of how we can select a GNSS navigation device for our drone with the above mentioned criteria and saw a few examples of the GNSS devices that are widely used in the drone industry to aid the GNSS for the drone.

# Summary

In this chapter, we have learned about the theory and selection of the key parts of the drone and the criteria that can be selected for them from the propulsion system to the GNSS device. This is the key concept of making a blueprint for the drone on all the parts available.

In the next chapter, we will focus more on the assembly and integration of these parts and the configuration software used for flying.

# Part 2: System Conceptualization and Avionics Development

Now that we have been introduced to the anatomy of a drone, that is, a quadcopter, in this part, we will study drones in a more detailed manner, including how each part works and how the quadcopter works. We will learn how variations in each part influence the working and dynamics of the quadcopter in mid-air. This part will be more detailed and analytical, with mathematical concepts and terminology to develop the system as a whole in software and hardware.

This part has the following chapters:

- Chapter 5: Sensors and IMUs with Their Application in Drones
- Chapter 6: Introduction to Drone Firmware and Flight Stacks
- Chapter 7: Introduction to Ground Control Station Software
- Chapter 8: Understanding Flight Modes and Mission Planning

## Chapter 5: The Application of Sensors and IMUs in Drones

In this chapter, we will learn and understand the major sensors that are used in drones to lift into the air and help their systems fly and navigate to waypoints. With the help of these sensors, the drone can fly in various modes and various patterns in manual and autonomous modes. We will learn about the working of these sensors in this chapter, how they interface, and what output they give. We will also learn about the impact they have on the drone's system. At the end of this chapter, we will be diving deep into the anatomy of a drone, which helps us to understand how it works.

We will cover the major sensors required in the drone system under the following topics:

- The inertial measurement unit and its role
- The barometer and its role
- GPS and magnetometer and their roles
- Voltage and current sensors and their roles
- Sensor fusion and state estimation

By going through the above topics, we will briefly be able to understand what the role of each sensor is, and this knowledge will be helpful in deriving the necessary equation and programming of the drone in that aspect.

### The Inertial Measurement Unit and Its Role

The **inertial measurement unit** (**IMU**) is the most important part of the drone as it helps the drone calculate its linear and angular acceleration; it also helps to measure the attitude angles across `roll`, `pitch`, and `yaw` maneuver, helping the autopilot make decisions and guide its motors and actuators to produce the desired output for level flight.

#### Composition of an IMU

The IMU is a critical component in multicopter flight control systems. It integrates multiple sensors to provide comprehensive information about the vehicle's motion and orientation. The IMU typically includes accelerometers, gyroscopes, and sometimes magnetometers. The integration of these sensors is essential for accurate flight control, stability, and navigation.

Components of the IMU:

- **Accelerometers**: These measure linear acceleration along different axes (`X`, `Y`, and `Z`)
- **Gyroscopes**: These measure the angular velocity or the rate of rotation around different axes
- **Magnetometers** (optional): These measure the strength and direction of the magnetic field

An IMU consists of the sensors mentioned in the following list, which are primarily present in the flight controller, meaning they are helpful in determining vehicle position and orientation. However, some IMUs come with the magnetometer built-in, and for some devices, we need to add this externally. The following are the main sensors used in the IMU:

- **Accelerometer**: This measures the acceleration along its axis.
- **Gyroscope**: This measures the angular velocity and acceleration.
- **Magnetometer**: This is an optional sensor in an IMU; some IMUs do come with the magnetometer, and some do not. A flight controller mostly comes with an accelerometer and gyroscope; however, a barometer and magnetometer are optional.

For proper flight, a drone requires IMU. The drone's internal sense of balance, or IMU, keeps it level and facilitates smooth flight. The IMU provides data to the drone's computer, which utilizes it to determine its motion. It functions similarly to the drone's means of ensuring that it maintains control and complies with the user's instructions. The data processing of the IMU enables the drone to fly precisely and perform as intended.

##### Accelerometer

An accelerometer measures acceleration, that is, the rate of a change in speed, to understand the changes in speed and direction. It helps in stability, helping flight controllers maintain flight levels and respond to pilot commands. The data from all three axes of an accelerometer are processed by the flight controller to compute the rate of change in the speed of the drone.

By measuring acceleration along each axis, the flight controller can integrate these data over time to calculate changes in velocity. Essentially, the accelerometer provides information about the drone's acceleration or deceleration rate in different directions, allowing the flight controller to make real-time adjustments to maintain stability.

Examples of MEMS accelerometer devices used in flight controllers:

- STMicroelectronics LSM6DS3
- InvenSense MPU-6050
- Bosch Sensortec BMI160
- ADXL345

The following is an image of a MEMS accelerometer:

![Figure 5.1 – MEMS Accelerometer](image/B19566_05_1.jpg)

Figure 5.1 – MEMS Accelerometer

A standard accelerometer measures the force of gravity on it across an axis. A standard accelerometer on Earth (at rest) would measure an acceleration of `9.8 m/s²`. When the system is in motion, it gives the acceleration data, which helps derive the speed, movement, and position of the drone.

##### Gyroscope

A gyroscope sensor measures the rate of rotation or angular velocity around different axes. In a drone, gyroscopes help to determine the drone's orientation and provide stability during flight. The data from the gyroscope helps the flight controller to understand how and in which direction the drone is turning and allows it to make real-time adjustments to maintain a level flight by adjusting the motor speed. For example, when a drone tilts or rotates to one side, the gyroscope data informs the flight controller, who then adjusts the motor speed on that side to counteract the movement and keep the drone stable and leveled:

![Figure 5.2 – Mechanical three-axis gyroscope](image/B19566_05_2.jpg)

Figure 5.2 – Mechanical three-axis gyroscope
