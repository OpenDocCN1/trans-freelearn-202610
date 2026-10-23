

![](img/b10cb271ed113c7ba5bfded0a75d13e0_508_0.png)

Figure 12.8 – A front view of a pressure transmitter (Credit: Showlight Technologies Ltd.: www.showlight.com.ng)

The following figure shows a simple connection to a pressure transmitter to produce a signal of 4 to 20 mA as the pressure varies:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_508_1.png)

Figure 12.9 – A connection to a pressure transmitter (Credit: Showlight Technologies Ltd.: [www.showlight.com.ng](http://www.showlight.com.ng))

The ammeter can be replaced with the analog input of a PLC that supports the current input (4 to 20 mA) if the signal will be fed to a PLC to perform a control function. If the analog input of the PLC only supports a voltage input (0 to 10 V), there will be a need to convert the current input of 4 to 20 mA to a voltage by connecting a 500-Ohm resistor across the signal. The last section of this chapter demonstrates this with a practical example.

You’ve now learned about pressure measurement and transmitters. Let’s proceed to learn about level measurement and transmitters in the following section.

## Level measurement and transmitters

The accurate measurement of levels is required in industry to maintain the proper fluid levels for safe operation, quality production, or the smooth running of equipment. Various sensor technologies are available for measuring levels in liquid and also in solids. Capacitance level sensors measure the change in capacitance to determine liquid levels. An ultrasonic level sensor emits a sound wave. The liquid level is directly proportional to the time delay between the wave emission and its reflection. Differential pressure sensor uses the difference in pressure between two points in a tank to determine the level. Level sensors mostly have a transmitter integrated that enables them to produce a standard signal of 4 to 20 mA or 0 to 10 V as a level varies.

There are various level transmitters. The following figure shows a hydrostatic level transmitter that is usually immersed in water or some liquid whose level is to be measured:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_510_0.png)

Figure 12.10 – A hydrostatic level transmitter (Credit: Showlight Technologies Ltd.: www.showlight.com.ng)

The following figure shows the wiring of a level transmitter that will produce an output of 4 to 20 mA as the level of water varies:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_510_1.png)

Figure 12.11 – The wiring of a level transmitter that will produce an output of 4 to 20 mA as the level of water varies (Credit: Showlight Technologies Ltd.: [www.showlight.com.ng](http://www.showlight.com.ng))

The ammeter can be replaced with an analog input of PLC that supports the current input (4 to 20 mA) if the signal will be fed to a PLC to perform a control function. If the analog input of the PLC only supports a voltage input (0 to 10 V), there will be a need to convert the 4 to 20 mA input to a voltage by connecting a 500-Ohm resistor across the signal. The last section of this chapter demonstrates this with a practical example.

You have now learned about level measurement and transmitters. Let’s proceed to the next section to learn about flow measurement and transmitters.

## Flow measurement and transmitters

Flow measurement is an operation carried out to measure the flow of fluids in a pipe using an instrument. Several flow-measuring instruments exist, which include the following:

- **A differential pressure flow meter:** This uses a pressure difference created by a sensing element (for example, an orifice plate or a Venturi tube) to determine the rate of a flow. A differential pressure transmitter has a high- and low-pressure port to which the high- and low-pressure side of the sensing element will be connected and it produces a standard signal of 4 to 20 mA that corresponds to the flow rate.

The following figure shows a Rosemount differential pressure transmitter:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_512_0.png)

Figure 12.12 – A Rosemount differential pressure transmitter (this file is licensed under the Creative Commons Attribution-Share Alike 4.0 International license)

- **A velocity flow meter**: This determines a flow rate by measuring the flow velocity. Velocity flow meters include the following:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_514_0.png)

Figure 12.14 – A Coriolis mass flow meter (this file is licensed under the Creative Commons Attribution-Share Alike 4.0 International license)

- *A thermal mass flow meter:* This relates the mass flow rate to the quantity of energy transferred from a heated probe to the flowing fluid. It is suitable for measuring gases. *Figure 12.15* shows a thermal mass flow meter.

![](img/b10cb271ed113c7ba5bfded0a75d13e0_515_0.png)

Figure 12.15 – A thermal mass flow meter (this file is licensed under the Creative Commons Attribution-Share Alike 4.0 International license)

We have just learned about flow meters (differential pressure, velocity, and mass flow meters), which should give you a basic idea of flow measurement. Let’s proceed to the following section to compound our knowledge about process control.

## Understanding the process control loop

The process control loop is a set of devices and tools designed to maintain the desired output from a process variable. Control loop components and instruments measure the variable, respond to it, and control it to maintain a setpoint.

A control loop system can be either an open-loop or closed-loop system, which we explained in an earlier section of this chapter. It usually consists of a sensor or transmitter, a controller, and an actuator. The following figure shows a process control loop for level control:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_516_0.png)

Figure 12.16 – The process control loop for level control

The process control loop in the preceding figure is explained briefly as follows: the pump supplies water to the tank and a control valve (the final control element) controls the flow rate depending on what it learns about the tank level through the signal it receives from the level controller, which has a setpoint. The level transmitter provides a feedback signal to the level controller. The level controller compares the feedback signal from the level transmitter to the setpoint and decides the control signal to give to the control valve to ensure an appropriate flow rate depending on the level of the liquid in the tank.

## A practical example of single-loop process control

Here, we will use a temperature sensor, a temperature controller, and a contactor (actuator) to keep the temperature of a heater at the desired level (or setpoint).

### Materials/devices

The required materials include the following:

- A thermocouple temperature sensor (Type K)
- A temperature controller (JTC903)
- A contactor
- A heating element

### Wiring

Make the connection as shown in the following diagram (*Figure 12.17*):

- this will energize the contactor because the A2 coil terminal of the contactor is connected to Neutral (N) of the AC supply while the A1 coil terminal that is connected to terminal 6 will now be on Live (L).

4. When the temperature of the heater is equal to the setpoint, the common (C) terminal of the temperature controller (terminal 7) will be disconnected from terminal 6 and connected to terminal 8. This will de-energize the coil of the contactor and power will no longer flow to the heater. Hence, the heater cools gradually and as it falls below 100, the coil is energized again and the process repeats.

This is a simple control that is not very effective, as you cannot maintain the exact setpoint because there will always be an overshoot. A better controller to use is a **Proportional-Integral-Derivative (PID)** temperature controller, which can be properly tuned to maintain the desired temperature.

In this section, we have practically demonstrated a simple process control for temperature. You can do this on your own by sourcing the required devices and making the connections as provided in the wiring (*Figure 12.17*). This should give you hands-on experience with a simple process control system.

In the following section, we will learn how to wire and program the analog input of a Siemens PLC to control a process.

## Wiring and programming the analog input of a PLC (Siemens S7 1200) for process control (hands-on)

In this section, we are going to dig deeper into our PLC wiring and programming by learning how to wire and program the analog input of PLC to control a process. Analog programming is often seen as the complicated aspect of using a PLC. I have made it simple and easy to understand here.

