
Here, in this section, we will look into the connection (wiring) of input devices (switches) and output devices (lights and actuators) to the PLC. **Sinking** and **sourcing** are two important terms in PLC wiring. They are used to describe the direction of the conventional current flow between two circuits or devices. A conventional current always flows from positive potential (+) to negative potential (-). Anytime you have a current flowing between two devices, one will be sourcing while the other will be sinking.

The sourcing device is the device that provides the current (positive), while the sinking device is the device that absorbs the current (that is, the device connected to negative or ground).

In PLC wiring, the two devices in question are input devices and input modules or output devices and output modules. PLC manufacturers usually produce input/output modules that you can either sink or source. There are PLCs where you can only sink their input/output module, and for some, you can sink or source it. It all depends on the manufacturer’s design. You are advised to consult the manual of the input or output module before wiring, regardless of the information you have gathered in this book.

Many PLC manufacturers usually have a common terminal on their PLC’s input or output module which must be connected to positive (e.g. if the module will be sourcing) or connected to negative (e.g. if the module will be sinking). When the common terminal of the input/output module is connected to the positive (+24V) of the power supply, we say the input/output module is sourcing, and when that common terminal is connected to the negative (- or 0V) of the power supply, we say the input/output module is sinking.

We are going to follow simple rules in wiring PLCs in this book:

- Sourcing input devices (switches and sensors) must be connected to a sinking input module. The following diagram shows the connection of sourcing input devices (**normally open (NO)** push button, **normally closed (NC)** push button, and limit and float switches) to a sinking input module:

Figure 7.11 – Connection of sinking input devices to a sourcing input module

Sourcing output devices (lights and actuators) must be connected to a sinking output module. The following circuit diagram shows the connection of sourcing output devices (pilot lamp, relay, contactor, and solenoid valve) to a sinking output module:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_242_0.png)

## Figure 7.12 – Connection of sourcing output devices to a sinking output module

• Sinking output devices (lights and actuators) must be connected to a sourcing output module. The following circuit diagram shows the connection of sinking output devices (pilot lamp, relay, contactor, and solenoid valve) to a sourcing output module:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_243_0.png)

## Figure 7.13 – Connection of sinking output devices to a sourcing output module

The preceding rules and wiring diagrams apply to PLCs whose input or output module is DC. The same concept goes for a PLC whose input or output module is AC, except that the terms *sinking* and *sourcing* will not be used. You can use **live (L)** and **neutral (N)** to describe the connection of the devices. For example, when an input device is connected to L, the input module must be connected to N, and vice versa. Also, when an output device is connected to L, the output module must be connected to N, and vice versa.

The following circuit diagram shows input devices (NO push button and NC push button) connected to L, while the input module is connected to N:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_245_0.png)

Figure 7.14 – Input devices connected to live (L), while the input module is connected to neutral (N)

The following circuit diagram shows input devices (NO push button and NC push button) connected to N, while the input module is connected to L:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_247_0.png)

Figure 7.16 – Output devices connected to live (L), while the output module is connected to neutral (N)

The following circuit diagram shows output devices (pilot lamp, relay, contactor, and solenoid valve) connected to N, while the output module is connected to L:

diagram shows the wiring of a DC output module that’s wired to a 220V AC contactor and also to a three-phase motor AC motor:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_249_0.png)

Figure 7.18 – Wiring of a DC output module that’s wired to a 220V AC contactor and also to a three-phase AC motor

In this section, we discussed various connections (wiring) that can be carried out on a PLC. The terms sinking and sourcing were thoroughly explained. You should now be able to connect sourcing input devices to a sinking input module, connect sinking input devices to a sourcing input module, connect sourcing output devices to a sinking output module, and connect sinking output devices to a sourcing output module. This section also explained how to connect input/output devices to input/output modules that use AC, and we finally showed a diagram that will help in understanding how to connect a DC output module to an AC load (*Figure 7.20*).

Let’s now proceed to the next section to advance our knowledge of PLC wiring.

## PLC wiring (2) – Wiring of proximity sensors (3-wire) to a PLC

In *Chapter 2, Switches and Sensors – Working Principles, Applications, and Wiring*, we learned about proximity sensors (capacitive and inductive). We will learn how to connect them to a PLC in this section.

Capacitive and inductive proximity sensors can be PNP or NPN. Either NPN or PNP type can come as NO or NC.

The following circuit diagram shows a 3-wire PNP (NO) and NPN (NO) proximity sensor wiring diagram:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_250_0.png)

Figure 7.19 – 3-wire PNP (NO) and NPN (NO) proximity sensor wiring diagram

Now, let’s differentiate between PNP and NPN sensors.

![](img/b10cb271ed113c7ba5bfded0a75d13e0_252_0.png)

Figure 7.20 – PNP sensor (NO) connection to a PLC

Let’s check the NPN sensor.

## NPN sensor

An NPN sensor, on the other hand, can be referred to as a sinking sensor. Recall that a sinking input device absorbs current, so an NPN sensor switches to negative when an object is detected. It is used with a sourcing input module (that is, a module that is connected to positive). The following circuit diagram shows an NPN sensor (NO) connection to a PLC:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_253_0.png)

Figure 7.21 – NPN sensor (NO) connection to PLC

We have now been able to differentiate between PNP and NPN proximity sensors and how to wire each. Let’s have a look at the pictorial diagram of the NPN sensor connection to a digital input module for better understanding:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_254_0.png)

Figure 7.22 – Pictorial diagram of an NPN sensor connection to a digital input module

The credit for this figure goes to Industrial Design Simulation Automation Services Ltd. and Showlight Technologies Ltd.

In this section, you have learned about the basic connection (wiring) of proximity sensors (3-wire) to a PLC. You should now be able to connect a PNP or NPN proximity sensor to a PLC.

We will now proceed to the next section to learn about the connection (wiring) of a photoelectric sensor to a PLC.

## PLC wiring (3) – Wiring of photoelectric sensors (retro-reflective)

We also discussed photoelectric sensors in *Chapter 2*, *Switches and Sensors – Working Principles, Applications, and Wiring*; in this section, we will learn how to wire a photoelectric sensor to an input module.

*Figure 2.49* in *Chapter 2* shows the wiring diagram of a retro-reflective photoelectric sensor.

The retro-reflective photoelectric sensor described in *Chapter 2* can be made to switch to positive or negative depending on whether the input module to be used with it is sinking or sourcing. If the **White (WH)** is connected to positive, the **Black (BK)** will have an output (+ve) when an object is detected, and if the WH is connected to negative, the BK will have negative when an object is detected.

The following circuit diagram shows the wiring of a photoelectric sensor connection to a sinking input module:
