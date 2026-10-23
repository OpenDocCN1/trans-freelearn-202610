
The following diagram shows how to use the sensor to turn a DC lamp on or off automatically by connecting the common (WH) to the positive:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_80_0.png)

Figure 2.50 – A retro-reflective photoelectric sensor being used to turn a DC lamp on or off (WH on positive)

In this section, you learned about some of the various proximity sensors that are used in the industry. The operation principles, symbols, and simple wirings that you learned about in this section will help you have a better understanding of industrial automation.

## Summary

Congratulations on completing this chapter! You should now be able to explain switches and sensors, describe manually operated switches and their various types, describe mechanically operated switches and their various types, and describe proximity sensors and their various types. You should also be able to identify the symbols for various switches and understand how they are wired to perform control functions.

The topics that were covered in this chapter are relevant for automation engineers and will help you understand the next chapter.

## Questions

The following are questions to test your understanding of chapter two. Ensure you have read and understood the topics in this chapter before attempting the questions.

- 1. ______________ provide information about the environment for a controller to process.
- 2. Devices that detect the presence or absence of object are referred to as _____________
- 3. In human being, eyes, nose, ear, tongue, and skin can be referred to as _____________
- 4. Switches that are operated by hand are referred to as _____________
- 5. A ______________ switch is controlled automatically by factors such as pressure, position, or temperature.
- 6. ________________ is a type of switch that has only one input (usually referred to as common) and can connect to and switch between two different outputs.
- 7. ________________ can detect the presence of liquids, powder, or granulated materials in a vessel.
- 8. A non-contact sensor that detects ferrous metals, such as carbon steel, stainless steel, and cast iron, is called ________________
- 9. A ________________ is a switch that opens or closes when temperature reaches a set-point or falls below a set point.
- 10. A switch that is operated by the motion of a machine part is called ________________
- 11. The symbol in Figure 2.51 represents ________________

![](img/b10cb271ed113c7ba5bfded0a75d13e0_82_0.png)

Figure 2.51

12. The symbol in Figure 2.52 represents ________________

![](img/b10cb271ed113c7ba5bfded0a75d13e0_82_1.png)

Figure 2.52

## Actuators and Their Applications in Industrial Automation

In the previous chapter, we related the way humans function to automation. The eyes, touch, and smell were likened to switches and sensors while the controller was likened to the brain. In this chapter, we will be looking at actuators, which can be likened to the hands. In our bodily system, the hands do what the brain tells them to do. Similarly, in automation, the actuator does what the controller tells it to do. An actuator is simply a mover. It can move, carry, or perform other similar functions just like the hand. It requires a control signal from a controller to carry out its actions. The control signal is the result of the processing done by the controller through the program (a set of instructions) written into it. Industrial automation is not complete without an actuator.

This chapter will explain various actuators used in industrial automation. You will learn their basic principle of operation and about their application in industry. Actuators are one of the key components in industrial automation that need to be given enough attention. This chapter will provide relevant information that an automation engineer needs to know about actuators.

In this chapter, we are going to cover these main topics:

- Introducing actuators
- Learning about electrical actuators
- Exploring pneumatic actuators
- Getting to know about hydraulic actuators

### Introducing actuators

An **actuator** is a device or component that produces an action or motion. It can be referred to as a **mover** because it moves to control a system. It usually requires a control signal from a controller and an energy source, which can be **electric**, **pneumatic** (air), or **hydraulic** (liquid). When it receives a control signal, it converts its own energy (electric, pneumatic, or hydraulic) to a form of energy or motion that can perform the required action.

One major application of an actuator in industry is the opening and closing of **valves**.

Manually operated valves use hand-wheels, manual gearboxes, knobs, and so on as their actuators for operation. The hand-wheel or knob can be turned clockwise or counterclockwise by humans to open or close the valve.

In situations where manual actuation of a valve to open or close it is not realistic, such as remote locations or on very large valves, a pneumatic, electric, or hydraulic actuator is required.

The actuator (pneumatic, electric, or hydraulic) provides the power or motion required to open or close the valve by converting its own energy to a mechanical force (or motion) when it receives a control signal from a controller. Figure 3.16 shows a valve with a hand-wheel on the left side of the figure that can be operated manually and a valve on the right side of the image with a pneumatic actuator that can be operated automatically via a control signal.

Figure 3.1 shows the functional block diagram of an actuator:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_85_0.png)

Figure 3.1 – Functional block diagram of an actuator

The motion produced by an actuator can either be linear or rotational. There are various types of actuators whose produced motion can either be linear or rotational.

There are basically three types of actuators:

- Electric actuators
- Pneumatic actuators
- Hydraulic actuators

We will look at what these three types of actuators are in the next topics of the chapter. We will learn about their basic construction as well as their application in industry. Having this knowledge will help in choosing the right actuator for your industrial automation needs.

### Learning about electric actuators

**Electric actuators** are used in industry to convert electrical energy to kinetic energy to cause the movement of loads or perform actions that required motion or force. The operation of most electric actuators is based on the interaction between a magnetic field and a current carrying a

conductor to create a *turning force*. Industrial fans, blowers, pumps, dampers, and so on are common applications of such an electric actuator. They utilize electric motors to perform the necessary action. Electric actuators are used in robotics and other assembly applications due to their accuracy, flexibility, and low operating cost. They are used in industrial machines that require circular motion. They are also used for clamping, pressing, cutting, stamping, and other forms of applications that require linear motion. An electric actuator can be referred to as a device that converts electrical energy into a linear or circular motion. Hence, electric actuators are of two types:

- **Electric linear actuator:** This is an actuator that will move in a linear direction. In this, rotational motion is first generated by an electric motor at high speed. It is then reduced by a gearbox to increase the torque, which will turn a leadscrew in a rotational motion, which in turn makes a drive nut move linearly. Limit switches are used to ensure the drive nut does not move beyond a certain point in the forward or reverse direction.

*Figure 3.2 shows the parts of a linear actuator:*

![](img/b10cb271ed113c7ba5bfded0a75d13e0_87_0.png)

Figure 3.2 – Parts of an electric linear actuator

Electric rotary actuator: Electric rotary actuators are commonly used in automation systems. Examples include direct current (DC) motors, AC motors, stepper motors, and servo motors.

We will look at examples of electric rotary actuators in the next sections.

### Direct current motors

Direct current (DC) motors convert electrical energy (through DC) to mechanical energy (motion). A DC motor consists of a stator, an armature, split ring commutators, and brushes. The stator is the stationary part, which usually consists of permanent magnets, and the armature is the rotating part, consisting of a coil of wire. The windings of the coil are connected to split
