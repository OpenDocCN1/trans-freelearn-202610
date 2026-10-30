![](bbox=[0.32, 0.14, 0.68, 0.39])

Here is the Python code for a basic turning operation:

```
# Home position
def setHome(ser):
    for i in range(0, 9):
        setAngle(ser, i, 90)
        setAngle(ser, 4, 85)

def setHomeNoHips(ser):
    for i in range(0, 3):
        setAngle(ser, i, 90)
    for i in range(5, 8):
        setAngle(ser, i, 90)

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
    setAngle(ser, 1, 90)

def turnLeftLegLeftCenter(ser):
    setAngle(ser, 9, 80)

def turnRightLegRightCenter(ser):
    setAngle(ser, 9, 100)

ser = serial.Serial("/dev/ttyACM0", 9600)
```