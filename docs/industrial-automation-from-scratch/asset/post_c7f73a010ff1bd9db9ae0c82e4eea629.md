
![](img/b10cb271ed113c7ba5bfded0a75d13e0_328_0.png)

Figure 9.24 – Simulation – Simulation result of network 2 when I0.2 is marked (ON)

Unmark the bit for I0.2 and mark the bit for I0.3. Q0.1 will still turn ON, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_328_1.png)

Figure 9.25 – Simulation – Simulation result of network 2 when I0.2 is unmarked and I0.3 is marked

Watch how the signal (colored green) flows from left to right to turn ON the coil (output) as you turn ON the input.

To end the simulation and continue writing the program, click on the Monitoring on/off icon to switch off the monitor mode. Click Yes:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_329_0.png)

Figure 9.26 – Confirmation message to go offline

You can continue writing or editing the program using the instructions (normally open contact, normally closed contact, and assignment/coil) explained in the previous section.

Perform the following steps to simulate other changes you have made to your program:
- 1. Click Compile and then click Download to device.
- 2. Click Load and then click Monitoring on/off to switch on the monitor mode.
- 3. On your simulator’s project view, add the input or output addresses you have used in the program as we did previously.
- 4. Test your program by marking or unmarking the bit for the input address you want to turn on or off.

We have seen how to simulate programs using Siemens TIA Portal. Next, we will understand what the latching and unlatching techniques are in PLC programming.

## Latching and unlatching in PLC programming

Latching is a technique used to keep an output energized or activated even when the input ceases. Latching makes a momentary push-button act as a maintained switch; in other words, after the push button is pressed, the output is turned on to remain energized (ON) even when the button is released.

Unlatching is simply a method or technique used to de-energize a latched output.

## A latch and unlatch program using normally open push buttons for both starting and stopping

Write the simple program in network 3 to see how latching and unlatching works with a normally open push button for starting and a normally open push button for stopping:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_330_0.png)

Figure 9.27 – A latch and unlatch program using normally open push buttons for both starting and stopping

The PLC wiring for the program is as follows:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_331_0.png)

Figure 9.28 – Wiring to implement a latch and unlatch program using normally open push buttons for both starting and stopping

The preceding wiring diagram shows the start push button (normally open) connected to I0.4, and the stop push button (normally open) connected to I0.5. A load, which can be the coil of a contactor, is connected to Q0.2.

When the start button is pressed, the normally open contact with the address I0.4 in the program will be closed. Power will flow to energize the coil with the address Q0.2 because I0.5 in the program is a normally closed contact. When the start button is released, the normally open contact with the address I0.4 in the program opens, but the output with the address Q0.2 remains energized due to the normally open contact with the same address as the output (Q0.2) that is parallel to I0.4. Coil Q0.2, which is energized (ON), closes the normally open contact with the address Q0.2 in parallel to I0.4. This creates an alternative path for the signal to flow to Q0.2. Hence, the output coil with the address Q0.2 remains ON even when the button is released. This is referred to as latching.

When the stop push button is pressed, the normally closed contact with the address I0.5 is opened. Power will no longer flow to the coil. The output coil with the address Q0.2 will become de-energized (OFF). This is referred to as unlatching.

The stop push button used here is unsafe because when the push button fails, the output will remain energized and the machine will keep running. This is not preferred and is unsafe for industrial use.

## A latch and unlatch program using a normally open push button for starting and a normally closed push button for stopping

Edit the program in network 3 such that the normally closed contact used for I0.5 will be a normally open contact, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_333_0.png)

Figure 9.29 – A latch and unlatch program using a normally open push button for starting and a normally closed push button for stopping

The PLC wiring for the program is as follows:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_334_0.png)

Figure 9.30 – Wiring to implement a latch and unlatch program with a normally open push button for starting and a normally closed push button for stopping

The preceding wiring diagram shows the start push button (normally open) connected to I0.4, and the stop push button (normally closed) connected to I0.5. A load, which can be the coil of a contactor, is connected to Q0.2.

When the start button is pressed, the normally open contact with the address I0.4 in the program will be closed. The stop push button (normally closed push button) will close the normally open contact with the address I0.5 in the program. Power will flow to energize the coil with the address Q0.2 because address I0.5 in the program is closed due to the normally closed push button used in the wiring. When the start button is released, the normally open contact with the address I0.4 in the program is opened, but the output with the address Q0.2 remains energized due to the normally open contact with the same address as the output (Q0.2) that is parallel to I0.4 in the program. Coil Q0.2, which is energized (ON), closes the normally open contact with the address Q0.2 that is parallel to I0.4 in the program. This creates an alternative path for the signal to flow to Q0.2. Hence, the output coil with the address Q0.2 remains ON even when the button is released. This is referred to as latching.

When the stop push button is pressed, the normally closed contact with the address I0.5 is opened. Power will no longer flow to the coil. The output coil with the address Q0.2 will become de-energized (OFF). This is referred to as unlatching.

The stop push button used here is safe because when the push button fails, the output will be de-energized and the machine that the stop push button is connected to will not run. This is preferred and safe for industrial use.

### Note

If the output (Q0.2) will be used to power on an induction motor, an overload relay is necessary between the contactor and the motor to ensure the motor stops when there is an overload. In this case, the normally closed contact (95-96) will be wired as input to the PLC, and a normally open contact will be used for it in the program (in series with the normally open contact for the stop) such that, when there is an overload, the normally open contact (95-96) opens and the motor will stop. This idea is used to start and stop an induction motor using PLC.

We have now learned about latching and unlatching. The two terms are important in PLC programming. We will now proceed to learn how to use an output address for an input in a program in the next section.

## Using an output address as an input in a program

In the previous chapter, we learned that an output address can be used as an input address (NC or NO contact) in the rules for ladder diagram programming.

The following example shows an output address (Q0.2) being used for an input. You can write the program in network 4 and simulate the program to see how it works.

![](img/b10cb271ed113c7ba5bfded0a75d13e0_336_0.png)

Figure 9.31 – Output address (Q0.2) used for a normally open (NO) contact at the input

The wiring diagram for the program is as follows:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_337_0.png)

Figure 9.32 – Wiring diagram to implement a program using an output address for input

Let’s say a pilot lamp (PL3) is connected to Q0.3, as shown in the preceding diagram.

Anytime Q0.2 is ON, the normally open contact with the address Q0.2 will be closed since the coil with the address Q0.2 is energized and PL3
