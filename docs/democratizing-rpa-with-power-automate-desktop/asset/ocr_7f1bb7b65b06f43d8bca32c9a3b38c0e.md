![](bbox=[0.126, 0.091, 0.874, 0.562])

# Figure 8.12 – The “Open terminal session” action parameters

+   - **Move cursor on the terminal session:** Sometimes, the cursor needs to be placed in a specific position to enter some data. This action can be used to place the cursor in the desired position by providing a value for the row and column number.
+   - **Get/set text from or to the terminal session:** These actions are used to enter or read data into the mainframe program – for example, the data of an order to be entered, including the order positions, could be entered with `set text`. Then, later, an automatically created order number could be read out with `get text`.
+   - **Send key to terminal session:** This action is used to perform a control within the terminal emulation – for example, by sending the `Transmit`