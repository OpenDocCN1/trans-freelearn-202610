
Figure 7.23 – Photoelectric sensor connection to a sinking input module

The following circuit diagram shows a photoelectric sensor connection to a sourcing input module:

Figure 7.24 – Photoelectric sensor connection to a sourcing input module

In this section, you have learned about the basic connection of a photoelectric sensor to a PLC. You should now be able to connect a photoelectric sensor to a sinking or sourcing input module of a PLC.

Let’s proceed to the next section to learn how to wire a PLC from one of the top PLC manufacturers.

![](img/b10cb271ed113c7ba5bfded0a75d13e0_257_0.png)

## Wiring of the Siemens S7-1200 PLC (CPU 1211C AC/DC/Relay)

The PLC wiring explained in this chapter provides the basic skill needed to get PLCs wired to sensors, lights, and actuators. It is advisable to consult the manual of any PLC you want to wire and follow the wiring/guides provided in the manual. The following diagram shows the wiring of the Siemens S7-1200 PLC (CPU 1211C AC/DC/Relay) in the manual:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_258_0.png)

Figure 7.25 – Siemens S7 1200 PLC wiring (CPU 1211C AC/DC/Relay)

When the NC push button connected to I0.1 is pressed, the corresponding indicator light (I0.1) will be OFF, and when the button is released, the corresponding indicator light (I0.1) will be ON.

The operation of the connected output devices (contactor and lamp) is a function of the program written into the PLC, which is covered in *Chapter 8*, *Understanding PLC Software and Programming with TIA Portal*.

When the written program is executed and the result of the logic energizes or activates Q0.0, the corresponding indicator light on the PLC (Q0.0) will be ON and the contactor will be energized. Similarly, when the result of the logic energizes or activates Q0.1, the corresponding indicator light on the PLC (Q0.1) will be ON and the lamp will be ON.

PLC wiring and programming work together. You need to understand the PLC wiring covered in this chapter to have a better understanding of the programming covered in *Chapter 8*.

You have just completed the PLC wiring part of this book. You should now be able to connect push buttons, limit switches, proximity sensors, lights, relays, contactors, solenoid valves, and other input or output devices to a PLC.

## Summary

Congratulations! You have successfully completed *Chapter 7* of this book. Good job! The chapter gives a good explanation of what you need to know about PLCs starting from the basic to the more advanced part involving wiring. You should now have a good understanding of what a PLC really is, the basic types available, PLC modules, and various wiring that you may need to carry out on PLCs as an industrial automation engineer. This chapter focuses on the hardware part of PLC.

The next chapter focuses on the software part, where you will learn how to program PLCs to perform automated tasks.

## Questions

The following are questions to test your understanding of this chapter. Ensure you have read and understood the topics in this chapter before attempting the questions:

- 1. ______________ is an industrial computer that consists of both hardware and software used for automating industrial processes.
- 2. PLC is an acronym for ________________________.
- 3. The basic modules that make up a PLC are: ________________________, ________________________, and ________________________.
- 4. ________________________ can be regarded as the brain of the PLC system.
- 5. The module or part that connects all the input devices (switches and sensors) to the PLC is called ________________.
- 6. The module or part that connects the PLC to the output devices or actuators is called ________________.
- 7. ________________ is the cycle in which the PLC reads the inputs, runs the PLC programs, performs diagnostic and communication tasks, and updates the output.
- 8. The time taken by the PLC to complete one scan cycle is referred to as ________________.
- 9. ________________ and ________________ are terms used to describe the direction of conventional current flow between two circuits or devices.

## Understanding PLC Software and Programming with TIA Portal

In the previous chapter, we learned about the hardware part of the **Programmable Logic Controller (PLC)**, which includes wiring the PLC to sensors and actuators. A PLC wired to switches, sensors, and actuators will do nothing unless there is a program written into it. A program (a set of instructions written using a programming language) is required for the CPU of the PLC to make decisions based on the inputs and carry out the required control function. It is important for an industrial automation engineer to have knowledge of both the hardware part of the PLC, including the wiring, and the software part, which includes programming.

This chapter focuses on the software aspect of the PLC. You will learn the basic things you need to start programming PLCs with the most common PLC programming language, **Ladder Diagram (LD)**. You will learn how to download, install, and use **Totally Integrated Automation Portal (TIA Portal)**, powerful software for programming Siemens PLCs.

We are going to cover the following main topics in this chapter:

- Understanding software/program in PLCs
- Introducing PLC programming languages
- Introducing PLC programming devices
- Understanding the basics of LD
- Downloading and installing TIA Portal V13 Professional and PLCSIM
- Creating a project and writing a program with Siemens programming software (TIA Portal)

## Technical requirements

While every part of this book is valuable, *[Chapter 2, Switches and Sensors – Working Principles, Applications, and Wiring](#)*, *[Chapter 3, Actuators and Their Applications in Industrial Automation](#)*, and *[Chapter 7, Understanding PLC Hardware and Wiring](#)*, are very important to have a better understanding of this chapter.

## Understanding software/program in PLCs

Before looking into PLC programming languages, let's get some knowledge of the software/program in PLCs.

In computing, generally, software is a set of instructions written in a language the computer understands to perform a task. Software can also be referred to as a program.

A PLC requires two types of software that must be executed by the CPU for the PLC to function. They are as follows:

- The operating system (firmware)
- User program

The operating system is the program written by the PLC manufacturers and designed to run automatically in the PLC once powered. It is responsible for executing the user program, establishing communication between devices, memory management, and updating the output.

A PLC user program is a set of instructions in either textual or graphical form that represents the control function that will be carried out for a specific industrial task or application. It is the program that the user or PLC programmer writes to dictate the required automation or control task to the PLC.

A programming language is usually used to convey instructions to a PLC or any other computing device to perform a specific task. Let's have a look at

PLC programming languages in the next section.

## Introducing PLC programming languages

According to **International Electrotechnical Commission (IEC)** standards, there are five programming languages, stated in part 3 of IEC 61131, for writing a program for PLCs to control and automate a task. These programming languages are the following:

- Ladder Diagram (LD)
- Function Block Diagram (FBD)
- Sequential Function Chart (SFC)
- Instruction List (IL)
- Structured Text (ST)

IEC 61131-3 can be purchased at their web store at the following link. However, there is an abstract you can read and also a preview that can be downloaded for free:

https://webstore.iec.ch/publication/4552

Note

IEC is an international standards organization responsible for preparing and publishing international standards for all electrical, electronics, and other related technologies.

Let's discuss each of these programming languages in detail.

## Ladder Diagram (LD)
