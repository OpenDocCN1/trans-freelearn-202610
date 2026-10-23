Let’s go online to monitor what happens in the PLC as it processes the analog signal and executes the program. Follow these steps to monitor your PLC program as it executes the instructions:

+   1. Double-click **Program blocks | Main [OB1]**.\n2. Click **Monitoring on/off**.

# An observation and explanation of the program

You will observe that the value at `%IW64` varies when the heater is on. This is the digitized value of the analog signal connected to the first channel of the analog input module. As the temperature increases, the analog signal increases and the digitized value increases as well. The value at `MD4` is the normalized value while the value at `MD8` is the scaled value.

In the following figure, **Network 1** shows the digitized value at `%IW64` is `9215`. The normalized value at `MD4` is `0.3332972` while the scaled value at `MD8` is `99.98915`.