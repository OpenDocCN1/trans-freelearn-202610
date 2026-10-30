```
else:
    print('Button Not Pressed')
GPIO.output(7, False)
```

![](bbox=[0.317, 0.205, 0.685, 0.525])

Now, save the code as `prog2.py` and run it with the following command:

```
python prog2.py
```

In this section, we configure pin #12 (by board numbering) to be an output via the same `GPIO.setup()` statement that we used in the previous section. But here, we have an extra argument, `pull_up_down=GPIO.PUD_UP`. This means that the internal resistor for the GPIO pin is being pulled up to 3.3V, and it will return `False` when the button is pressed. This is a little counter-intuitive, but without pulling up or down, the input will be left "floating", which means that the Pi will not be able to determine a button press simply because it does not have anything to compare the voltage level to!

Now we enter the infinite `while` loop, where it checks for the button state using the `GPIO.input()` function. As mentioned earlier, if the button is pressed, `input_state` will return `False` and the LED will be lit up. If, instead, the button is not pressed anymore, the LED will go off!

Simple, isn't it? Let's move on a more complicated add-on.