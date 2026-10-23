
![](img/b10cb271ed113c7ba5bfded0a75d13e0_138_0.png)

Figure 4.18 – Simplified circuit diagram of a compound wound DC motor

Now that we have an understanding of various brushed DC motors, let's get an understanding of brushless DC motors.

## Brushless DC motors

This design does not contain brushes. Hence, there is no wear and tear of brushes as experienced in a brushed type of DC motor. The stator (armature) consists of windings of coil through which current is passed to produce a magnetic field. The rotor is made of a permanent magnet unlike the brushed type.

When a DC power is applied to the stator, it becomes an electromagnet. The force and reaction between the permanent magnet in the rotor and the electromagnet created in the stator produces the required rotation. A common example of a brushless DC motor is the type used in the fan of a computer power supply, as shown in the following figure:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_139_0.png)

Figure 4.19 – Brushless DC motor. Left – rotor with a permanent magnet and blade. Right – stator showing coil windings

The credit for this photo goes to *Wikimedia Commons* and can be found at the following link: https://creativecommons.org/licenses/by-sa/4.0/deed.en.

In this section, we learned about DC motors. We discussed the two common types of DC motors, that is, the brush and brushless types. The brushed type of DC motor was further classified as a permanent magnet, series wound, shunt wound, and compound wound. DC motors are also used for industrial automation. Hence, it will be wise for any automation engineer to have knowledge of their basic structure.

Let's now learn about stepper motors and servo motors.

## Understanding stepper motors and servo motors

A stepper motor can be referred to as a type of brushless DC motor that divides its full rotation into a number of equal steps and rotates in steps.

They have multiple coils organized in phases. The motor will rotate a step at a time as each phase is energized in sequence.

Basically, stepper motors are of two types:

- Unipolar
- Bipolar

Let's look at them in detail in the following sections.

### Unipolar stepper motors

This usually has six wires, but sometimes it has five, as shown in the following diagram. They have one winding with a center tap per phase:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_140_0.png)

Figure 4.20 – Unipolar stepper motor

Let's have a look at a bipolar stepper motor.

### Bipolar stepper motors

These usually have four wires. They also have one winding per phase but do not have a center tap. They have more torque and are more efficient than unipolar stepper motors:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_141_0.png)

Figure 4.21 – Bipolar stepper motor

Let's now learn how to drive a stepper motor.

### Driving a stepper motor

Unlike a regular brushed DC motor that can be connected directly to a DC source, driving a stepper motor is more complicated. It usually requires a stepper motor driver or stepper motor controller to energize the phases in a timely sequence to get it turned.

The following diagram shows a simple stepper motor driver that can be used for a bipolar stepper motor:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_142_0.png)

Figure 4.22 – Stepper motor driver

The four leads or wires of the stepper motor are connected to terminals A+, A-, B+, and B- and the power supply is connected to VCC and GND.

DIR +, DIR -, PUL +, and PUL - can be connected to a PLC to send the pulse and direction signal as shown in Figure 4.23:

A servo motor is also not simple to control like a DC motor that can just be connected to a DC source. A basic DC servo motor can be controlled with a microcontroller. AC servo motors used in industry usually require a PLC and a servo amplifier (driver) as shown in the following diagram:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_144_0.png)

Figure 4.24 – Simplified wiring of PLC, servo motor driver, and servo motor

In this section, we have learned about stepper motors and servo motors. They are special types of motors used in industrial automation. Unlike the basic DC motor that only needs to be connected to a DC source to get it running, stepper motors and servo motors require a driver to get them running. They are very useful in the field of robotics and industrial automation. Hence, the importance of knowledge of stepper motors and servo motors cannot be over-emphasized for an industrial automation engineer.

## Summary

Congratulations! You have successfully completed another chapter of this book. Well done. The chapter explained electric motors as machines used to convert electrical energy into mechanical energy. Classifying electric motors based on the type of power supply they use, we have AC motors and DC motors. AC motors can further be classified into synchronous and asynchronous while DC motors can be further classified as brushed and brushless.

You should now understand asynchronous motors and be able to connect in a star or delta connection. Also, you should be able to wire an asynchronous motor to start and stop it using the DOL starter method.

The chapter also discussed stepper motors and servo motors. You should now understand stepper motors and how to drive them. And finally, you should now have an idea of how a servo motor can be connected to a PLC.

The knowledge acquired in this chapter will help in understanding **Variable Frequency Drive (VFD)** in the next chapter.

## Questions

The following are questions to test your understanding of this chapter. Ensure you have read and understood the topics in this chapter before attempting the questions:

- 1. The two types of electric motor when classified by their source of power are __________ and __________.
- 2. A motor whose rotor speed is the same as the speed of the rotating magnetic field of the stator is called __________.
- 3. __________ is the phenomenon in which current is induced in a conductor placed in a changing magnetic field.
- 4. In an AC induction motor, the difference between the speed of the rotating magnetic field of the stator and the speed of the rotor is called __________.
- 5. A type of DC motor whose field winding (housed in the stator) is in series with the armature winding is called __________.
- 6. A type of brushless DC motor that divides its full rotation into a number of equal steps and rotates in steps is called

## Introduction to Variable Frequency Drive (VFD)

In the previous chapter, we learned about the various types of electric motors. Among the various types, the asynchronous (induction) motor is the most commonly used motor in the industry. However, machines in the industry, such as conveyors, mixers, agitators, pumps, compressors, and others, that utilize an induction motor require a variable speed, which an induction motor alone cannot provide because induction motors are designed to run at a constant speed (that is, their speed remains almost the same for various load range).

A variable speed drive is an electronic device used to achieve the varying speed needed by either an asynchronous or synchronous motor for various applications.

This chapter looks at a Variable Frequency Drive (VFD), its working principle, benefit, wiring, and programming. We will also look at some practical applications, including forward, reverse, and speed control of an induction motor.

In this chapter, we are going to cover the following main topics:

- An overview of a VFD
- Wiring a VFD
- Understanding a basic VFD parameter setting
- Wiring and programming a VFD to start/stop a three-phase motor and setting the frequency (speed) using a keypad
- Wiring and programming a VFD (VAT20) to use a push button or switch to run a three-phase motor in a forward/reverse direction and to use a keypad to set the frequency (speed)
