# Accessing the GPIO Pins on Raspberry Pi Zero

+   2. Now reboot Raspberry Pi; you can use the smbus library ability to read and write from your I2C device. Fortunately, in this case the device comes with some example code on how to configure the device and then read a value's. Here is an example of a Python code:

![](bbox=[0.175, 0.224, 0.823, 0.719])

```
```python
#!/usr/bin/python
import smbus
import time
import math
bus = smbus.SMBus(1)
address = 0x1e

def read_byte(adr):
    return bus.read_byte_data(address, adr)
def read_word(adr):
    high = bus.read_byte_data(address, adr)
    low = bus.read_byte_data(address, adr+1)
    val = (high << 8) + low
    return val
def read_word_2c(adr):
    val = read_word(adr)
    if (val >= 0x8000):
        return -((65535 - val) + 1)
    else:
        return val
def write_byte(adr, value):
    bus.write_byte_data(address, adr, value)

write_byte(0, 0b01110000) # Set to 8 samples @ 15Hz
write_byte(1, 0b00100000) # 1.3 gain LSB / Gauss 1090 (default)
write_byte(2, 0b00000000) # Continuous sampling
scale = 0.92
x_out = read_word_2c(3) * scale
y_out = read_word_2c(7) * scale
z_out = read_word_2c(5) * scale
bearing = math.atan2(y_out, x_out)
if (bearing < 0):
    bearing += 2 * math.pi
print "Bearing: ", math.degrees(bearing)
```
```