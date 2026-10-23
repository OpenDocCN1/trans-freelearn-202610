cap the bottle. The capping machine will stop after 4 seconds due to the normally closed contact (timer2) that is in series with the coil with the address Q0.2 opening when timer2 is energized after 4 seconds.

+   - **Network 5:** When the normally open contact with the address M0.0 is closed due to the start button being pressed and released, and the normally open contact with the address I0.4 is closed due to the job out sensor detecting a bottle, a counter (counter1) will start. The counter will increment each time a bottle passes.

+   - **Network 6:** When the normally open contact with the address M0.0 is closed due to the start button being pressed and released, and the counter in network 5 has incremented to 6, the normally open contact (counter1) will close and since timer3 is a normally closed contact, the coil with the address Q0.3 will be energized and a wrapping machine connected to Q0.3 will start to wrap the 6 bottles. A 5-second timer (timer3) also starts when counter1 is incremented to 6.

After 5 seconds, the normally closed contact (timer3) will open and the wrapping machine connected to Q0.3 will stop. The counter (counter1) in network 5 will also reset after the 5 seconds of timer3 have elapsed so that the next bottle will be counted as 1 and keep incrementing up to 6, when the wrapping machine will start to wrap again.

The program for an automated filling, capping, and wrapping system learned in this section covers almost all the instructions we have learned so far in this book. Try to write and simulate the program on your own to gain more knowledge regarding the practical application of the instructions we have learned in the previous chapter and also in this chapter for programming PLCs with ladder diagram programming language.

# Summary

Congratulations! You have successfully completed another chapter of this book. Good job! PLC is the brain behind most modern automated machines and programming is key when automating a machine with PLC. You should now understand latching and unlatching, know how to use an output