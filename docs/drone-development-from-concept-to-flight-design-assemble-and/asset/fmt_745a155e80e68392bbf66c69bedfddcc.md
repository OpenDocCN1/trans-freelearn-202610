
# Drone Classification by Weight

*   **Nano**: Any drones less than or equal to 250 grams come under the category of nano drones
*   **Micro**: Any drones between 250 grams and 2 kilograms come under the category of micro drones
*   **Small**: Any drones between 2 kilograms and 25 kilograms come under the category of small drones
*   **Medium**: Drones that are greater than 25 kilograms and less than or equal to 150 kilograms
*   **Large**: Drones that are greater than 150 kilograms

We have now studied different types of drones based on their structure and application, but we haven't yet studied what's actually inside the drone and the systems it contains. In the following sections, we will understand the composition of a drone and bifurcate it into different categories.

# System Composition of a UAV

A UAV has many systems and subsystems that enable it to fly in the air and do missions automatically with safety and precision. These systems are a combination of hardware and software that perform their respective tasks to keep the system under control and stable in the air:

![Figure 1.11 – Overview of a drone system](image/B19566_01_11.jpg)

Figure 1.11 – Overview of a drone system

Now that we have seen the system composition of a UAV, let us look at the major mechanical and structural components of drones.

# Major Mechanical and Structural Components of a Drone

A drone system is a robotic system that is composed of electro-mechanical systems for all its functions. A mechanical system is called a skeleton, the drone under which all the avionics system works. The mechanical system holds the avionics system firmly with it with appropriate strength so that it can take maneuver forces upon it to its limits.

We will study here the major mechanical and structural components of a drone, which are required to hold different parts and have their independent functionalities.

## Airframe

The airframe is the main skeleton of a drone, which holds all avionics components in position and helps them to be mounted and fit firmly without any vibrations and loose fitting during the flight. It works as the main body of the drone, which gives the system a proper shape and size, confines all modules, and protects them from direct exposure to the external environment:

![Figure 1.12 – A hexacopter carbon fiber airframe](image/B19566_01_12.jpg)

Figure 1.12 – A hexacopter carbon fiber airframe

A complete airframe is composed of the following subcomponents:

### Motor Mounts

Places to hold the motors using screws or other materials:

![Figure 1.13 – A motor mount](image/B19566_01_13.jpg)

Figure 1.13 – A motor mount

### Arms

Tubes/pipes between the main body and motor mounts are called arms. These are used as a stiff mechanical structure to lift the main body and wiring between motors and the main body:

![Figure 1.14 – An arm set](image/B19566_01_14.jpg)

Figure 1.14 – An arm set

### Hub

The hub is the place where the main avionics, such as flight controllers, the **Global Positioning System** (**GPS**), and other components, are placed with due interfacing and connection, which helps the system to get the necessary data to process. Arms are attached to the hub and extended outside:

![Figure 1.15 – A drone hub](image/B19566_01_15.jpg)

Figure 1.15 – A drone hub

### Landing Gear

This is also attached to the hub extending downward. This helps the drone to land on different terrains and also keeps adequate ground clearance for the safety of the payload:

![Figure 1.16 – A landing gear](image/B19566_01_16.jpg)

Figure 1.16 – A landing gear

In terms of the features of a mechanical airframe, the following is recommended:

*   The airframe should be symmetrical from all aspects on the `x`, `y`, and `z` axes
*   Manufacturing of the airframe is to be done from lightweight materials such as carbon fiber, glass fiber, and the like
*   A screwless design would be even more helpful for stability and performance

# Avionics Systems and Subsystems of Drones

Post the mechanical system comes the avionics system, which is fitted into the mechanical system at appropriate required places to exert thrust and other forces, keeping the center of gravity balanced. These avionics systems include sensors and actuators powered by the power system to exert the necessary force where required and get the necessary task done by the drone:

![Figure 1.17 – Avionics components](image/B19566_01_17.jpg)

Figure 1.17 – Avionics components

In the following sections, we will have a glance at the major avionics systems and subsystems of a drone that are interrelated.

## Propulsion System (Drive Train)

The propulsion system is responsible for creating the necessary force to lift the system into the air, maintaining the ratios of lift to weight. This is also a reason to maintain stability in the air, fly at high altitudes, and provide speed to the system. This is the most important part and initial building block of drone systems. It also helps in maintaining electrical stability in the system:

![Figure 1.18 – A power train](image/B19566_01_18.jpg)

Figure 1.18 – A power train

It consists of the following primary parts:

### Motors

These are the primary components of the propulsion system. These are **brushless DC** (**BLDC**) motors, which are far more efficient and lower-power-consumption motors with a long life. These motors rotate at high speed with propellers and produce the necessary thrust to lift the system in the air. Most of the power of the battery is consumed by these motors. These motors are the primary actuators to create motion in the 3D space, travel from one position to another, and lift loads:

![Figure 1.19 – A BLDC motor](image/B19566_01_19.jpg)

Figure 1.19 – A BLDC motor

### Propellers

Propellers are key thrust generators in the multirotor. These are mounted on the motors using screws or other mechanisms. Propellers are manufactured with lightweight materials in such a way that when rotated at high speed, they push the air downward, produce thrust, and lift the system upward. Each propeller produces a set amount of thrust when rotated at a desired RPM, which we will see in the coming chapters:

![Figure 1.20 – Propellers](image/B19566_01_20.jpg)

Figure 1.20 – Propellers

## Power System

The power system is the system that is responsible for powering the whole drone with its payload and other peripheral devices. This is the key system that keeps a drone powered, helps in getting the required endurance, and nullifies the effects of high wind or heavy maneuvers. Certain buck-and-boost converters and filters are added to step up and down voltages when required, and current consumption is greater.

These are the major components of the power system of drones:

### Battery

The battery is the most important part of the system. It serves as a key source of power delivery to the system. All systems and subsystems are powered by the battery. The battery must be capable of delivering power as required by the system without failure and serve the current delivery. It is also responsible for deciding the endurance of the drone:

![Figure 1.21 – A battery](image/B19566_01_21.jpg)

Figure 1.21 – A battery

### Power Distribution Board

The power distribution board, as the name suggests, helps to distribute power to different systems. High-power-consumption elements such as motors take power through this board, and it is evenly distributed among them. A few more filtering elements are added, such as a capacitor to prevent any part from burning due to surges and extra load current:

![Figure 1.22 – A power distribution board](image/B19566_01_22.jpg)

Figure 1.22 – A power distribution board
