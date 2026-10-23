
![](img/b10cb271ed113c7ba5bfded0a75d13e0_547_0.png)

Figure 12.47 — Deleting the tags for MD0 and Q0.0

3. Now, input the address of the first analog input channel (that is, `%IW64`) as the value for `NORM_X` in **Network 1**. Also, input a memory bit address (that is, `M0.0`) as the address for the output in **Network 2**. You should see something similar to the following figure:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_547_1.png)

Figure 12.48 – %IW64 now used as the value for NORM_X and M0.0 now used as the register for Assignment (coil)

+   4. Double-click the `50.0` value at the bottom of the **compare** instruction and input `100` to change the set point to 100:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_548_0.png)

Figure 12.49 – 50 changed to 100

+   5. Expand **Bit logic operations** under the **Basic instructions** list on the right-hand side of the screen and drag the normally closed instruction to **Network 3**:

Figure 12.50 – The normally closed contact instruction added to Network 3

6. Now, input %M0.0 in the ??? field of the normally closed contact:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_549_0.png)

Figure 12.51 – The normally closed contact now has a M0.0 register

7. Expand Bit logic operations under the Basic instructions list on the right-hand side of the screen and drag the assignment (Coil)

# instruction to Network 3:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_550_0.png)

Figure 12.52 – The Assignment (Coil) instruction added to Network 3

8. Now, input %Q0.0 in the ??? field of assignment (coil) to get something similar to the following figure:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_550_1.png)

Figure 12.53 – Assignment (Coil) given an address of Q0.0

We have just completed the program. The following screenshot shows the complete program from Network 1 to Network 3:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_551_0.png)

Figure 12.54 – The complete program (an analog input program)

## Configuring the IP address and downloading the program to a real PLC

Now, let’s proceed to download the program to a real PLC:

+   1. Ensure all the connections in the wiring diagram (Figure 12.45) have been set up correctly. Switch on the circuit breaker to supply power to the devices. Also, ensure PLCSIM (simulator) is not running.
+   2. Connect one end of the communication cable (the Ethernet cable) to your laptop or PC and connect the other end to the PLC. Figure 12.55 here shows my simple setup for demonstration purposes:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_552_0.png)

Figure 12.55 – My setup for demonstration purposes

3. Double-click Device configuration:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_553_0.png)

Figure 12.56 — Device configuration

+   4. Navigate through the **Properties | General** tab and then click **PROFINET Interface [X1]**:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_553_1.png)

Figure 12.57 — PROFINET Interface [X1]

+   5. Scroll down to the IP protocol and input an IP address of your choice (for example, `192.168.0.6`) and a subnet mask (for example, `255.255.255.0`) as shown:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_554_0.png)

Figure 12.58 — Adding an IP address and a subnet mask

+   6. Click **Save project** and click on the PLC to select it. You should have the PLC highlighted. The **Compile** and **Download to device** commands will become active as shown here:

Figure 12.60 – The selected PG/PC interface

10. Click Load:

Figure 12.61 – The Extended download to device window

11. Click Yes:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_557_0.png)

Figure 12.62 – Extended download to device (message 1)

+   12. Click **OK**:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_557_1.png)

Figure 12.63 – Extended download to device (message 2)

+   13. Select **Stop all**:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_558_0.png)

Figure 12.64 – Load preview (1)

14. Click Load:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_558_1.png)

Figure 12.65 – Load preview (2)

15. Check Start all as shown here and click Finish:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_559_0.png)

Figure 12.66 – Load results

Congratulations! You have successfully downloaded the program to your PLC and the PLC will begin to function as programmed:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_559_1.png)

Figure 12.67 – Loading completed with no errors

Let’s go online to monitor what happens in the PLC as it processes the analog signal and executes the program. Follow these steps to monitor your PLC program as it executes the instructions:

+   1. Double-click **Program blocks | Main [OB1]**.
+   2. Click **Monitoring on/off**.

## An observation and explanation of the program

You will observe that the value at `%IW64` varies when the heater is on. This is the digitized value of the analog signal connected to the first channel of the analog input module. As the temperature increases, the analog signal increases and the digitized value increases as well. The value at `MD4` is the normalized value while the value at `MD8` is the scaled value.

In the following figure, **Network 1** shows the digitized value at `%IW64` is `9215`. The normalized value at `MD4` is `0.3332972` while the scaled value at `MD8` is `99.98915`.

MD8 is 105.1649.

In Network 2, there was an output in the comparator to energize or turn ON the M0.0 Assignment (Coil) because the condition is met, that is, 105.1649 at MD8 is greater than 100.

In Network 3, the normally closed contact (M0.0) becomes open because M0.0 is ON. This stops the signal from flowing to Q0.0, which de-energizes the contactor and the heater turns OFF.

![](img/b10cb271ed113c7ba5bfded0a75d13e0_562_0.png)

Figure 12.69 – Monitoring the PLC program as it executes instructions (2)

In this section, you have learned how to work with an analog input signal in a PLC. You should now be able to program and simulate a PLC (S7-1200) for analog input signal processing. You should also be able to connect a

transmitter with 4 to 20 mA output to an analog input of a real PLC and program it to perform a simple control (for example, stop a heater) at desired setpoint.

This section simplifies analog signal processing which many industrial automation engineers or PLC specialists finds difficult. This book only focuses on analog input signals and we have used it to send out a digital control signal when a condition is met. It is also possible to send out an analog signal (4 to 20 mA) or use a PLC with a PID feature for better control than the example here.

## Summary

Well done! You have successfully completed this chapter, which is an important stage of industrial automation. You should now understand process control and be able to explain various process control terms and understand ISA symbols according to *ISA5.1*. You have also learned about the sensors and transmitters required for temperature measurement, level measurement, pressure measurement, and flow measurement, which are common in industry. You should now be able to wire a temperature sensor and transmitter, level transmitter, and pressure transmitter to generate a 4-to 20-mA analog output, which is very important for process control. We have also learned how to keep the temperature of a heater at the desired level by using a temperature controller.

The last section covered more advanced topics within the field of industrial automation, where we discussed the basic theory you need to know about analog input signal processing in a PLC (S7-1200), which includes reading analog signals, normalizing, and scaling. We learned how to program a PLC (S7-1200) to process analog input signals using a step-by-step guide (simulation) and we also used a real PLC to learn how to wire and program the analog input of a real PLC to perform a control action following a step-by-step guide.

The next chapter involves other vital knowledge you will need as an industrial automation engineer. Don’t miss out.
