
contact with the address I0.2 creates an alternative path for the signal to flow to the coil (Q0.0) to keep the pump running even when the low-level sensor detects liquid and the high-level sensor has not yet detected any liquid.

- **Network 3:** When M0.0 is energized (due to the start button being pressed and released) and the low-level and high-level sensors have not detected liquid, the normally open contact with the addresses I0.2 and I0.3 will be opened in the program, M0.1 will be de-energized and the pump keep running. M0.1 is used in rung 2 (network 2) to cause the pump to stop when energized. The normally open contact with the address M0.1 in rung 2 (network 2) will become open when the address M0.1 in rung 3 (network 3) is energized and this will cause the pump to stop running. This will happen when the tank is full.

The normally open contact with the address M0.1 in parallel with I0.3 is used to create an alternative path for the signal to flow to M0.1 when the manual discharge valve is open and the water level in the tank decreases gradually. As the water decreases, it will get to a point where it will fall below the high-level sensor, causing the normally open contact with the address I0.3 (which was closed when water was full) to open, and since we don’t want M0.1 to be de-energized to cause the pump to start immediately (in other words, we want it to start when the liquid level falls below the high- and low-level sensors), the address M0.1 is used to keep the coil M0.1 energized so that the pump can remain off until the liquid falls below the low-level sensor with the address I0.2. Recall from the operation that when the manual discharge valve is opened and the liquid falls below the high-level sensor, the pump should not start until the liquid falls below the low-level sensor.

- **Network 4:** This rung is responsible for the pump ON indicator. A normally open contact with the same address as the coil for the pump, in other words, Q0.0, is used as input, while a coil with the address Q0.1 is used in the output section of the rung. Anytime Q0.0 is energized (which must happen in order for the pump to run), the normally open contact with the address Q0.0 will close and Q0.1 will be energized.

> Hence, the indicator lamp connected to Q0.1 will turn on anytime the pump is running.

We’ve just learned a simple level control that can be used in the industry. This knowledge can help in developing more advanced level controls. We will see an automated filling, capping, and wrapping system using PLC in the next section.

## Automated filling, capping, and wrapping system using PLC

The following diagram shows another industrial application of PLC:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_359_0.png)

Figure 9.51 – Filling, capping, and wrapping

The start push button (normally open) is connected to I0.0, while the stop push button (normally closed) is connected to I0.1. A conveyor motor is connected to Q0.0, a fill valve is connected to Q0.1, a capping machine is connected to Q0.2, and a wrapping machine is connected to Q0.3. A fill position sensor is connected to I0.2 and a cap position sensor is connected

to I0.3, while a job out sensor is connected to I0.4, as indicated in the preceding diagram.

When the start push button is pressed and released, the conveyor motor should start running until the bottle gets to the fill position sensor; in other words, the fill position sensor detects the bottle and the conveyor motor stops.

The fill valve should open as soon as the conveyor motor stops at the fill sensor for 10 seconds to fill the bottle. After 10 seconds, the valve should close and the conveyor motor should continue running until the bottle gets to the cap position sensor, in other words, the cap position sensor detects the bottle and the conveyor motor stops.

The capping machine should start as soon as the conveyor motor stops at the cap sensor for 4 seconds to cap the bottle. After 4 seconds, the capping machine should stop and the conveyor motor should continue running.

When the bottle gets to the job out sensor, a counter should increment by one and the bottle should drop to the wrapping machine. When the number of bottles detected by the job out sensor reaches 6, in other words, when the counter increments to 6, the wrapping machine should start to wrap the 6 bottles and stop after 5 seconds.

The program can be written as follows:

Figure 9.52a – Filling, capping, and wrapping program

(a)

Figure 9.52b – Filling, capping, and wrapping program

(b)

Figure 9.52 – Filling, capping, and wrapping program

The different networks are explained as follows:

- **Network 1:** This is a simple start and stop program to start and stop the system. Refer to the explanation of network 1 in the previous section to understand how it works. When the start push button is pressed and released, M0.0 will be energized, and it will remain energized until the stop push button at I0.1 is pressed.
- **Network 2:** M0.0, which will be energized in network 1 when the start button is pressed and released, will cause the normally open contact with the address M0.0 to close, and since I0.2 (address of the fill position sensor) and I0.2 (address of the cap position sensor) are normally closed, Q0.0 (address of the conveyor motor) will be energized and the conveyor motor will start running. When the fill position sensor detects a bottle, it will turn ON and this will make I0.2 to open and cause the motor to stop at that position until timer1 (filling time) elapses and close the normally open contact (timer1) in parallel to I0.2 to create an alternative path for the signal to flow to coil Q0.0 to keep the conveyor running. When the cap position sensor detects a bottle, it will turn ON, this will make I0.3 to open and cause the motor to stop at that position until timer2 (capping time) elapses and close the normally open contact (timer2) in parallel to I0.3 to create an alternative path for the signal to flow to coil Q0.0 to keep the conveyor running.
- **Network 3:** When the normally open contact with the address M0.0 is closed due to the start button being pressed and released, and the normally open contact with the address I0.2 is closed due to the fill position sensor that detects a bottle, a 10-second timer (timer1) will start and, at the same time, an output with the address Q0.1 will be energized. Hence, the valve connected to Q0.1 will open to fill the bottle. The valve will close after 10 seconds due to the normally closed contact (timer1) that is in series with the coil with the address Q0.1 opening when timer1 is energized after 10 seconds.
- **Network 4:** When the normally open contact with the address M0.0 is closed due to the start button being pressed and released, and the normally open contact with the address I0.3 is closed due to the cap position sensor that detects a bottle, a 4-second timer (timer2) will start and, at the same time, an output with the address Q0.2 will be energized. Hence, the capping machine connected to Q0.2 will start to

cap the bottle. The capping machine will stop after 4 seconds due to the normally closed contact (timer2) that is in series with the coil with the address Q0.2 opening when timer2 is energized after 4 seconds.

- **Network 5:** When the normally open contact with the address M0.0 is closed due to the start button being pressed and released, and the normally open contact with the address I0.4 is closed due to the job out sensor detecting a bottle, a counter (counter1) will start. The counter will increment each time a bottle passes.

- **Network 6:** When the normally open contact with the address M0.0 is closed due to the start button being pressed and released, and the counter in network 5 has incremented to 6, the normally open contact (counter1) will close and since timer3 is a normally closed contact, the coil with the address Q0.3 will be energized and a wrapping machine connected to Q0.3 will start to wrap the 6 bottles. A 5-second timer (timer3) also starts when counter1 is incremented to 6.

After 5 seconds, the normally closed contact (timer3) will open and the wrapping machine connected to Q0.3 will stop. The counter (counter1) in network 5 will also reset after the 5 seconds of timer3 have elapsed so that the next bottle will be counted as 1 and keep incrementing up to 6, when the wrapping machine will start to wrap again.

The program for an automated filling, capping, and wrapping system learned in this section covers almost all the instructions we have learned so far in this book. Try to write and simulate the program on your own to gain more knowledge regarding the practical application of the instructions we have learned in the previous chapter and also in this chapter for programming PLCs with ladder diagram programming language.

## Summary

Congratulations! You have successfully completed another chapter of this book. Good job! PLC is the brain behind most modern automated machines and programming is key when automating a machine with PLC. You should now understand latching and unlatching, know how to use an output
