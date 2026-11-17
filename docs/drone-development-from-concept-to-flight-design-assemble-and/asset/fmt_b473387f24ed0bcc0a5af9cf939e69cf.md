
*   **RX protocols**: Communication between the radio **receiver** (**RX**) and **flight controller** (**FC**)
*   **TX protocols**: Communication between the radio **transmitter** (**TX**) and the radio RX

In the following figure, we can see the synchronization of a TX and an RC, as well as their binding with the flight controller:

![Figure 3.7 – How a TX and RX work with an FC](image/B19566_03_7.jpg)

Figure 3.7 – How a TX and RX work with an FC

## Transmitter protocols

Wireless communication is a method of communication that’s used between a radio TC and an RX. Many manufacturers offer their own TX protocols. Some brands even offer more than one protocol, depending on the hardware used. Here’s a list of the most common TX protocols that are used:

*   ACCST (Frsky)
*   ACCESS (Frsky)
*   DSM (Spektrum)
*   DSM2 (Spektrum)
*   DSMX (Spektrum)
*   AFHDS (Flysky)
*   AFHDS 2A (Flysky)
*   A-FHSS (Hitec)
*   FASST (Futaba)
*   Hi-Sky (Deviation / Devo)

These protocols have not been explained here because they are not widely used today and are not relevant to this book. These protocols are readily available online if you want a quick overview.

## Receiving protocols

The radio receiver communicates with the flight controller via a wire-wired interface. Some RX protocols are standard and found in all radio receivers from the same manufacturer, while others are proprietary to a specific manufacturer. The following are some of the standard RX protocols:

*   PPM or CPPM
*   PWMSBUS (Futaba, Frsky)
*   IBUS (Flysky)
*   XBUS (JR)
*   MSP (Multiwii)
*   CRSF (TBS Crossfire)
*   SPEKTRUM1024 (Spektrum DSM2)
*   SPEKTRUM2048 (Spektrum DSMX)
*   FPort (Frsky)

### Pulse position module (PPM)

PPM is also known as CPPM or PPMSUM. The PPM flag is a set of PWM signals that are sent randomly on a single balanced wire.

The advantage of PPM over PWM is that it has one cable for many channels. Therefore, in general, ground lines, power lines, and flag lines should be connected to eight channels.

It is not as accurate or smooth as serial traffic (even small traffic) because channel values do not arrive at the same time:

![Figure 3.8 – An image showing the three-pin cable connection of a PPM](image/B19566_03_8.jpg)

Figure 3.8 – An image showing the three-pin cable connection of a PPM

### Pulse width modulation (PWM)

PWM shares the characteristics of digital and analog signals in that the length of the pulse indicates the servo output or throttle position. Signals are usually between 1000 μs and 2000 μs (microseconds).

This is the most common and original radio control protocol. Since there is no flight controller, the receiver is used to directly control servos and ESCs with standard PWM signals.

The downside to this is that cabling is a problem because there is only one servo cable per channel. So, PPM and SBUS are usually better than PWM when using FC. FC works just as well – if not better – when you can get all the channels on one phone:

![Figure 3.9 – An RC receiver and PWM connection](image/B19566_03_9.jpg)

Figure 3.9 – An RC receiver and PWM connection

### Serial protocols

A serial protocol is a lossless digital protocol that uses only three wires (signal, power, and ground) for multiple channels. Unlike PPM, which signals in the time domain, the serial protocol is fully digital – that is, it uses 1s and 0s. As the name suggests, the serial protocol requires a serial port on the flight controller.

### SBUS protocol

SBUS is the same language that the drone and remote control use to communicate with each other. This is a digital way for the remote control to tell the drone what to do. Communication is easy and efficient because information is sent through a single phone instead of multiple phones. This helps the drone understand and follow the remote control commands so that it can fly smoothly.

# Summary

In this chapter, we learned about various command and control interfaces and what they mean for drones. We learned about the diverse types of ground control stations, how they work, and what they mean. We also learned about the protocols and procedures that are involved in RC transmitters and receivers, as well as how to use them. We now have a clear idea of what GCS is and why it is needed.

In the next chapter, you will learn how to design a drone from scratch, how to select each part of the drone, and how to create a successful drone.

# 4. Knowing UAV Systems, Sub-Systems, and Components

Earlier, we read about the forces that work on a drone, its major hardware, systems, and sub-systems, and the systems that are applicable to the drone. Now, we will go ahead by learning about the development of these hardware systems. In this chapter, we will learn about the major concepts, terminologies, and laws that are helpful in the development of drones as a whole. This would let the user design their own system as per the specifications given.

In this chapter, we will learn how to select the best components for the drone as per their specification requirements and how they can be integrated to develop a drone. We will go through the following topics to help you understand the concept in detail:

*   Understanding and conceptualizing the system
*   Designing the propulsion system
*   Designing and developing the airframe
*   Developing the control system
*   Selecting the navigation and communication system

## Technical requirements

In this chapter, the major technical requirements are basic high school physics, knowledge of the resultant forces that act on the body, and basic electrical engineering and calculations. It would be beneficial to have an understanding of circuit voltage, current, power, batteries, and so on. Experience of handling electrical instruments is a plus.

## Understanding and conceptualizing the system

Understanding and conceptualizing helps us to build every kind of drone for its dedicated application and purpose. As we saw in [*Chapter 1*](#B19566_01.xhtml#_idTextAnchor013) the major applications for which a drone can be used, we now see how drones can be conceptualized and developed.

A drone is basically a platform that can fly into the air with a certain amount of payload capacity, which we have already studied with **maximum take-off weight** (**MTOW**). Thus, a drone platform can be helpful in transforming a drone for different applications by applying the permissible payload to it and optimizing its weight, endurance, size, and power.

When we are conceptualizing a system, we need to think about and decide on the following things:

*   **The application**: The purpose of this is to know the application and purpose for which the drone is built, for example, an urban mapping by a LiDAR payload, a land survey using an RGB camera, crop spraying/seeding in agriculture, day and night surveillance, or a high endurance tether drone. Knowledge of the application would help you to conceptualize the system in a better way in terms of airframe design and selection and placement of components. You will be better aware of the kind of situation and the work the drone has to go through; thus, the drone can be customized in that way.
*   **Endurance (flight time)**: Endurance is the key factor that has to be considered while conceptualizing the drone. Long endurance helps you to fly longer and do more productive missions without landing frequently for battery replacements. However, the demand for the application is still regarded.
