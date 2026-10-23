The preceding wiring diagram shows the start push button (normally open) connected to I0.4, and the stop push button (normally open) connected to I0.5. A load, which can be the coil of a contactor, is connected to Q0.2.

When the start button is pressed, the normally open contact with the address I0.4 in the program will be closed. Power will flow to energize the coil with the address Q0.2 because I0.5 in the program is a normally closed contact. When the start button is released, the normally open contact with the address I0.4 in the program opens, but the output with the address Q0.2 remains energized due to the normally open contact with the same address as the output (Q0.2) that is parallel to I0.4. Coil Q0.2, which is energized (ON), closes the normally open contact with the address Q0.2 in parallel to I0.4. This creates an alternative path for the signal to flow to Q0.2. Hence, the output coil with the address Q0.2 remains ON even when the button is released. This is referred to as latching.

When the stop push button is pressed, the normally closed contact with the address I0.5 is opened. Power will no longer flow to the coil. The output coil with the address Q0.2 will become de-energized (OFF). This is referred to as unlatching.

The stop push button used here is unsafe because when the push button fails, the output will remain energized and the machine will keep running. This is not preferred and is unsafe for industrial use.

# A latch and unlatch program using a normally open push button for starting and a normally closed push button for stopping

Edit the program in network 3 such that the normally closed contact used for I0.5 will be a normally open contact, as shown in the following screenshot: