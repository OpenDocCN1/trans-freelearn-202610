14. In our `loop()` method, we are going to read the current state of the button and compare it to the last value we saved. If the button state changes from *pressed* to *released*, we are going to send a message to the serial bus:

```
void loop() {
  a = analogRead( 0 );
  if ( abs( a - lastA ) > 10 ) {
    Serial.print( "A" );
    Serial.println( a );
    lastA = a;
  }

  b = analogRead( 1 );
  if ( abs( b - lastB ) > 10 ) {
    Serial.print( "B" );
    Serial.println( b );
    lastB = b;
  }

  c = analogRead( 2 );
  if ( abs( c - lastC ) > 10 ) {
    Serial.print( "C" );
    Serial.println( c );
    lastC = c;
  }

  button = digitalRead( 2 );
  if ( buttonLast == HIGH && button == LOW ) {
    Serial.println( "click" );
  }

  buttonLast = button;
}
```

15. Compile and upload the code to your Arduino and switch to Processing.

16. Run our Processing sketch, turn the knobs, and press the button. Each knob sends its own message now, and when the button is pressed, we receive a message on the Processing side. This is what our Processing sketch looks like when a button is pressed:

![](bbox=[0.4, 0.692, 0.592, 0.829])