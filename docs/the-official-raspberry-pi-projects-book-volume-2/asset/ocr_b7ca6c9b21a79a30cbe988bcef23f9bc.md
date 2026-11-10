# PI ZERO Reset
RESET SWITCH

An easy first soldering project, adding a reset switch to your Pi Zero – or full-size Pi – is a great way to prevent crashes and hangs from ruining your day

# You'll Need

+   - Soldering iron & solder
+   - 2× 2.54mm male header pins
+   - Recycled PC case reset switch
+   Or
+   - Momentary switch, normally open magpi.cc/zdXrPZg
+   - 2× female jumper wires

![](bbox=[0.2, 0.37, 0.95, 0.65])

A reset switch salvaged from an old PC case is perfect for this project, coming with its wires already attached

Available on any modern Raspberry Pi, the RUN header can be used to recover from a crash or launch a shut-down Pi.

The Pi Zero’s diminutive size, low cost, and tiny power requirements mean it’s a great fit for embedded projects. There’s only one problem: how to quickly get things back up and running if your code hits an infinite loop or otherwise crashes the Pi Zero, especially in projects where the Pi Zero is locked away from prying hands and powered by a hard to interrupt internal battery. The solution? A reset switch, wired to the RUN header on the Pi Zero, which can restart the Pi in the event of a crash, or power it on from a shut-down state.

# >STEP-01
Find the RUN Header

Known as header P6 on early models, the RUN header is linked to a pin in the processor which keeps it in standard running mode – hence the name RUN. When pulled to ground by shorting one contact on the header to the other, the processor leaves the RUN state. To all intents and purposes, this has the same effect as briefly cutting the power to the processor; if the Pi was running, it will reboot, and if the Pi was shut down, it will power on. The RUN header can be found on the Pi Zero at the right-hand side of the board, sandwiched between the GPIO header and the composite video header (marked ‘TV’).

# >STEP-02
Insert the male headers

The RUN header is unpopulated in all Raspberry Pi models, meaning