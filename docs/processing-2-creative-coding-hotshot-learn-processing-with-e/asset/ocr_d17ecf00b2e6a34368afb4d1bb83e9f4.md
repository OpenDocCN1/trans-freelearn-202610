3. Now connect the Arduino pins, analog inputs 0, 1, and 2 to the middle pins of your resistors. Your wiring should look as shown in the following diagram:

![](bbox=[0.347, 0.193, 0.646, 0.548])

4. In our loop() method, we need to copy the code that reads the value and sends it to the serial port to read the other two ports as well:

```
int a = 0;
int lastA = a;

int b = 0;
int lastB = b;

int c = 0;
int lastC = c;

void setup() {
  Serial.begin( 9600 );
}
```