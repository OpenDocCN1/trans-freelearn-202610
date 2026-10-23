
- **Operation mode (F03):** This parameter allows you to specify the way you want the two push buttons connected to terminal 3 and terminal 4 to function. The two options available are as follows:
  - **Forward/stop and reverse/stop:** Value = 0
  - **Run/stop and forward/reverse:** Value = 1

Specifying 0 as the value for parameter F03 will make the push button or switch connected to terminal 3 function as forward/stop, while the push button or switch connected to terminal 4 will function as reverse/stop. Specifying 1 as the value for the parameter will make the push button or switch connected to terminal 3 function as run/stop, while the push button or switch connected to terminal 4 will function as forward/reverse.

- **Operation control (F10):** This parameter allows you to specify your choice of operating the VFD. The two available options are as follows:
  - **Keypad:** Value = 0
  - **External terminal:** Value = 1

Specifying 0 as the value for parameter F10 allows you to operate the VFD (run/stop) using the keypad or operator panel on the VFD. Specifying 1 as the value for the parameter allows you to operate the VFD using push buttons, switches, or via a signal from an external device (a PLC or controller).

- **Frequency control (F11):** This parameter allows you to specify a frequency source. The three options available are as follows:
  - **Keypad:** Value = 0
  - **External terminal (0–10V or 0–20mA):** Value = 1
  - **External terminal (4–20mA):** Value = 2

Specifying 0 as the value for parameter F11 allows you to set the frequency of the VFD using the keypad (operator panel). Specifying 1 as the value for the parameter allows you to set the frequency using a potentiometer connected to terminals 8, 9, and 10, as shown in the wiring (pictorial diagram) of Figure 5.9, or send a signal of 0–10V or 0–20mA from an external device. Specifying 2 as the value for the parameter allows you to set the frequency by sending a 4–20mA signal from an external device.

It is advisable to study the manual of any VFD you want to work with. We have learned about some VFD parameter settings of General Electric VFD (VAT 20). You can learn more from the manual of the VFD. Let’s get our hands dirty with a practical example. We will be learning how to wire and program the General Electric VFD (VAT 20) to start/stop a three-phase motor with a keypad and also use a keypad to set the frequency (speed).

## Wiring and programming a VFD to start/stop a three-phase motor and setting the frequency (speed) using a keypad

Before we start the programming, let’s wire the system, as shown in the following connection diagram:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_165_0.png)

Figure 5.11 – The keypad and display of a General Electric VFD (VAT20)

The credit for this photo goes to *Showlight Technologies Limited* www.showlight.com.ng.

Switch on the circuit breaker to supply power to the drive and start the programming by following the steps:

- 1. Press the **DSP/FUN** key to enter the parameter setting and use the up/down arrow to scroll to the **F10** parameter (operation control).
- 2. Press **DATA/ENT** for the parameter value and use the up/down arrow to scroll to **0** (keypad).
- 3. Press **DATA/ENT** to accept the value. The display will show **END** and return to **F10**.
- 4. Use the up/down arrow to scroll to the **F11** parameter (frequency control).
- 5. Press **DATA/ENT** for the parameter value and use the up/down arrow to scroll to 0 (keypad).
- 6. Press **DATA/ENT** to accept the value. The display will show **END** and return to **F11**.
- 7. Press **DSP/FUN** key to exit the parameter setting.

You have just completed the programming and will see the frequency blinking. Use the up or down arrow key to specify a frequency and press **RUN/STOP** to start or stop the motor.

We have now learned how to use a keypad to start or stop a three-phase motor. Let’s look at another practical example to further increase our knowledge.

## Wiring and programming a VFD (VAT20) to use a push button or switch to run a three-phase motor in a forward/reverse direction and use a keypad to set the frequency (speed)

Before we start the programming, let’s wire the system as shown in the following diagram:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_167_0.png)

Figure 5.12 – The wiring required to use a push button or switch to run a three-phase motor in a forward/reverse direction and use a keypad to set the frequency (speed)

The credit for this figure goes to *Showlight Technologies Limited* at www.showlight.com.ng.

Switch on the circuit breaker to supply power to the drive and start the programming by following the steps:

- 1. Press the **DSP/FUN** key to enter the parameter setting and use the up/down arrow to scroll to the **F03** parameter (operation mode).
- 2. Press **DATA/ENT** for the parameter value and use the up/down arrow to scroll to **0** (forward/stop and reverse/stop).
- 3. Press **DATA/ENT** to accept the value. The display will show **END** and return to **F03**.
- 4. Use the up/down arrow to scroll to the **F10** parameter (operation control).
- 5. Press **DATA/ENT** for the parameter value and use the up/down arrow to scroll to **1** (external terminal).
- 6. Press **DATA/ENT** to accept the value. The display will show **END** and return to **F10**.
- 7. Use the up/down arrow to scroll to the **F11** parameter (frequency control).
- 8. Press **DATA/ENT** for the parameter value and use the up/down arrow to scroll to **0** (keypad).
- 9. Press **DATA/ENT** to accept the value. The display will show **END** and return to **F11**.
- 10. Press **DSP/FUN** key to exit the parameter setting.

You have just completed the programming and will see the frequency blinking. Use the up or down arrow key to specify a frequency, and use the push button or switch connected to terminal 3 to run the motor in a forward direction or stop the motor. Use the push button or switch connected to terminal 4 to run the motor in a reverse direction or stop the motor.

We have now learned how to use a push button or switch to run a three-phase motor in a forward/reverse direction and use a keypad to set the frequency (speed). Let’s further increase our knowledge by adding a potentiometer to set the frequency (speed).

## Wiring and programming a VFD (VAT20) to use a push button or switch to run a three-phase motor in a forward/reverse direction and to use a potentiometer to set the frequency (speed)

Before we start the programming, let’s wire the system as shown in the following diagram:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_169_0.png)

Figure 5.13 – The wiring required to use a push button or switch to run a three-phase motor in a forward/reverse direction and to use a potentiometer to set the frequency (speed)

The credit for this figure goes to *Showlight Technologies Limited* at [www.showlight.com.ng](http://www.showlight.com.ng).

Switch on the circuit breaker to supply power to the drive and start the programming by following the steps:

- 1. Press the **DSP/FUN** key to enter the parameter setting and use the up/down arrow to scroll to the **F03** parameter (operation mode).
- 2. Press **DATA/ENT** for the parameter value and use the up/down arrow to scroll to **0** (forward/stop and reverse/stop).
- 3. Press **DATA/ENT** to accept the value. The display will show **END** and return to **F03**.
- 4. Use the up/down arrow to scroll to the **F10** parameter (operation control).
- 5. Press **DATA/ENT** for the parameter value and use the up/down arrow to scroll to **1** (external terminal).
- 6. Press **DATA/ENT** to accept the value. The display will show **END** and return to **F10**.
- 7. Use the up/down arrow to scroll to the **F11** parameter (frequency control).
- 8. Press **DATA/ENT** for the parameter value and use the up/down arrow to scroll to **1** (external terminal).
- 9. Press **DATA/ENT** to accept the value. The display will show **END** and return to **F11**.
- 10. Press **DSP/FUN** key to exit the parameter setting.

You have just completed the programming and will see the frequency blinking. Use the potentiometer to specify a frequency, and use the push button or switch connected to terminal 3 to run the motor in a forward direction or stop the motor. Use the push button or switch connected to terminal 4 to run the motor in a reverse direction or stop the motor.

## Summary

The chapter explained the VFD as a device that can be used to change the speed of an AC motor by varying its frequency and voltage. We have also learned about the advantages that made them popular in various industries. Also, we looked at the disadvantages that need to be known, understood, and mitigated. We also learned how to select a VFD, and finally, we learned how to wire and program a VFD (VAT 20) for some common applications.

In the next chapter, we will learn how to use **Computer-Aided Design (CAD)** software to draw electrical diagrams, which is very important for industrial automation engineers to communicate with each other.

## Questions
