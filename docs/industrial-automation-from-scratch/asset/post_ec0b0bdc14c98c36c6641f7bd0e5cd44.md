
## Switches and Sensors – Working Principles, Applications, and Wiring

Understanding how a human being functions will help you understand automation. Having a brain is not enough for a human being to operate. Sight, hearing, touch, and smell are required to provide information about the environment for the brain to process. Similarly, **switches and sensors** provide information about the environment for a controller to process. The result of processing determines the actions that will be carried out by an *actuator*, which will be discussed in the next chapter. This chapter is specifically reserved for switches and sensors to enable us to discuss actuators in depth. Just like sight, hearing, touch, and smell are important for humans to function, switches and sensors are important components that are required to automate an industrial process.

In this chapter, we are going to cover the following main topics:

- Introducing switches and sensors
- Describing manually operated switches
- Describing mechanically operated switches
- Understanding proximity sensors (capacitive, inductive, and photoelectric)

### Introducing switches and sensors

A **switch** is an electrical component that connects or disconnects a signal path in an electrical circuit. It is used to make or break an electrical circuit. A switch has two states: an ON state and an OFF state. In an electrical circuit, the current will flow when the switch is in the ON state. When in the OFF state, the current will not flow. Hence, the ON state will connect a signal path, while the OFF state will disconnect a signal path. A switch can be referred to as a device that's used to turn equipment ON or OFF.

Switches can be categorized as follows:

- Manually operated switch
- Mechanically operated switch

**Sensors** are devices that detect or sense the presence or absence of objects. A sensor can be thought of as an automatic switch. They gather information from their environment and convert it into a form of signal that can be read and/or seen by an observer or equipment. In a human being, their eyes detect light energy, their nose can detect chemicals or various kinds of smell, their ears can detect sound, and their skin can detect temperature or pressure. Likewise, in automation, various types of sensors detect various physical quantities such as light, sound, temperature, and so on. These physical quantities will be converted into a digital or analog signal that can be read and/or seen by an observer or piece of equipment (controller). There are various types of sensors, but we will cover proximity sensors later in this chapter.

In this section, we learned about switches and can summarize them as devices that are used to turn equipment either ON or OFF. We also learned that sensors can be thought of as automatic switches. They are not operated by humans. Having an idea of switches and sensors will give you a better understanding of the types of switches that will be explained in the next section. It will also help you understand proximity sensors, which will be explained later in this chapter.

### Describing manually operated switches

In manufacturing, there is always the need for an operator to start or stop a machine and there could be a need to manually perform some other form of control aside from starting and stopping. These usually require switches that can be operated by hand. Switches that are operated by hand are referred to as **manually operated switches**. Some examples are as follows:

- Push button
- Rocker switch
- Toggle switch
- Selector switch
- Knife switch

We will explore each of these in the following sections.

#### Push button

A **push button** is a type of switch that closes or opens a circuit when a button is pressed or depressed. The button is designed to operate in two states (that is, ON or OFF). When it's in the ON position, an internal metal spring contacts the two terminals of the switch, allowing the current to flow. When it's in the OFF position, the internal switch retracts and the current will stop flowing.

Push buttons exist as normally *open* or normally *closed*.

Normally open or normally closed buttons can be used to control a single electrical circuit.

In a normally open push button, when the push button is not pressed, the contacts are open, and the connection is broken (that is, the switch is OFF). When the push button is pressed, the contacts are closed, and the connection is made (that is, the switch is ON).

In a normally closed push button, when the push button is not pressed, the contacts are closed, and a connection is made (that is, the switch is ON). When the push button is pressed, the contacts are open, and the connection is broken (that is, the switch is OFF).

The following diagram shows the symbols for a **normally open (NO)** and **normally closed (NC)** push button:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_34_0.png)

NORMALLY OPEN (NO) PUSH BUTTON

![](img/b10cb271ed113c7ba5bfded0a75d13e0_34_1.png)

NORMALLY CLOSED (NC) PUSH BUTTON

Figure 2.1 – Symbols for normally open (NO) and normally closed (NC) push buttons

The following figure shows the front and rear of an NO push button. Terminal 3 and terminal 4 are used to connect the switch to a circuit:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_34_2.png)

![](img/b10cb271ed113c7ba5bfded0a75d13e0_34_3.png)

Figure 2.2 – The front and rear of an NO push button

The credit for this image goes to *Showlight Technologies LTD* [www.showlight.com.ng](http://www.showlight.com.ng).

The following wiring diagram shows the use of an NO push button to light a lamp. The lamp will come on when the push button is pressed and go off when it is released or depressed:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_35_0.png)

Figure 2.3 – Using an NO push button to light a lamp

The following figure shows the front and rear of an NC push button. Terminal 1 and terminal 2 are used to connect the switch to a circuit:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_36_0.png)

Figure 2.4 – The front and rear of an NC push button

The credit for this image goes to *Showlight Technologies LTD* [www.showlight.com.ng](http://www.showlight.com.ng).

#### Rocker switch

A **rocker switch** is an on and off switch in which one side is raised and the other side is depressed when pressed. It rocks like a rocking horse rocks back and forth. The following figure shows what it looks like from the front and rear view:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_37_0.png)

Figure 2.5 – The front and rear of a DPST rocker switch

The credit for this image goes to *Showlight Technologies LTD* [www.showlight.com.ng](http://www.showlight.com.ng).

Rocker switches are available in various forms:

- SPST
- SPDT
- DPST
- DPDT

Let's explore what they are, one by one.

**Single Pole Single Throw (SPST)** can switch (turn on or off) only one circuit. It is the simplest form of a switch in that it can only have one input and can connect to only one output.

The following diagram shows the symbol of an SPST switch:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_37_1.png)

Figure 2.6 – Symbol of an SPST

The following wiring diagram shows the simple wiring for an SPST switch to light a lamp:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_38_0.png)

Figure 2.7 – Simple wiring diagram showing the use of an SPST switch to light a lamp

**Single Pole Double Throw (SPDT)** is a type of switch that has only one input (usually referred to as common) and can connect to and switch between two different outputs. A contact is usually connected to the input by default, and this is called a normally closed contact. The other contact, which is connected to the input during operation, is referred to as a normally open contact.

The following diagram shows the symbol of an SPDT switch:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_39_0.png)

SINGLE POLE DOUBLE THROW (SPDT)

Figure 2.8 – Symbol of an SPDT switch

The following is a simple wiring diagram of an SPDT switch being used to light two lamps (one at a time):

![](img/b10cb271ed113c7ba5bfded0a75d13e0_39_1.png)

Figure 2.9 – Using an SPDT switch to light two lamps

**Double Pole, Single Throw (DPST)** is a type of switch that has two inputs (usually referred to as common) and each input can connect to and switch between one corresponding output. It consists of two SPST switches that operate simultaneously in one package.

The following diagram shows the symbol for a DPST switch:
