Here is the Python code for each of the functions for the different states:

```
def liftLeftLeg(ser):
    setAngle(ser, 5, 70)
    setAngle(ser, 0, 80)
    time.sleep(.5)
    setAngle(ser, 5, 90)
    setAngle(ser, 7, 110)
    setAngle(ser, 8, 80)
    setAngle(ser, 6, 105)

def liftRightLeg(ser):
    setAngle(ser, 0, 110)
    setAngle(ser, 5, 100)
    time.sleep(.1)
    setAngle(ser, 0, 90)
    setAngle(ser, 2, 70)
    setAngle(ser, 3, 100)
    setAngle(ser, 1, 95)

def stepLeftForward(ser):
    setAngle(ser, 8, 80)
    setAngle(ser, 7, 100)
    setAngle(ser, 6, 95)
    time.sleep(.5)
    setAngle(ser, 3, 110)
    time.sleep(.5)
    setAngle(ser, 2, 80)
    time.sleep(.5)
    setAngle(ser, 1, 105)
    setAngle(ser, 0, 90)
    time.sleep(.5)

def stepRightForward(ser):
    setAngle(ser, 3, 80)
    setAngle(ser, 2, 70)
    setAngle(ser, 1, 70)
    time.sleep(.5)
    setAngle(ser, 8, 80)
    time.sleep(.5)
    setAngle(ser, 7, 90)
    time.sleep(.5)
    setAngle(ser, 6, 75)
    time.sleep(.5)
    setAngle(ser, 5, 90)

ser = serial.Serial("/dev/ttyACM0", 9600)
```

You'll notice that each function has a number of different servo control statements; these must be performed in this order to get the desired result.