# Installing PiGlow

The following figure shows the PiGlow board:

![](bbox=[0.322, 0.204, 0.679, 0.506])

Since we have learned how to control an LED and a button via Raspberry Pi GPIO pins, we will now move on to a module called PiGlow, which is an add-on board for the Raspberry Pi and provides 18 individually controlled LEDs, and which you can use via a provided library. We will now proceed to install the software requirements for PiGlow by using a script provided by Pimoroni itself. To install PiGlow, run the following command in a new terminal:

```
```bash
curl get.pimoroni.com/piglow | bash
```
```

To check whether PiGlow has been installed properly, open a Python terminal and execute the following command:

```
```python
import piglow
```
```

If it is successful, then continue.

To confirm whether I2C has been enabled, open the Raspberry Pi configuration tool by entering the following command:

```
```bash
sudo raspi-config
```
```