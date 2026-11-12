

```} else if ( in.charAt( 0 ) == 'B' ) {
  mouth = int(in.substring( 1,in.length()-2 ));
} else if ( in.charAt( 0 ) == 'C' ) {
  col = int(in.substring( 1,in.length()-2 ));
}
}
```

- 6. Connect your Arduino board to the USB port and start your Smilie-O-Mat sketch. Each variable resistor controls one facial parameter.

- 7. Now we are going to add support for the button, so add the following code to the serialEvent() method to make it react to the click messages we send from our Arduino:

```
void serialEvent( Serial p ) {
  String in = p.readStringUntil( '\n' );
  if ( in != null ) {
    if ( in.charAt(0) == 'A' ) {
      eye = int(in.substring(1,in.length()-2));
    } else if ( in.charAt(0) == 'B' ) {
      mouth = int(in.substring(1,in.length()-2));
    } else if ( in.charAt(0) == 'C' ) {
      col = int(in.substring(1,in.length()-2));
    } else if ( "click".equals( in.trim() ) ) {
      tweet();
    }
  }
}
```

- 8. If we would run the code now, every now and then our Arduino would send two or three click messages in quick succession, because when a button closes or opens, there is a short phase where it has very little contact and switches very fast between the open and closed state. This behavior is called **bouncing**. To prevent multiple messages from being sent to Twitter, we are going to implement a little countermeasure in our Arduino sketch called **debouncing**. Open the Arduino sketch and add the following two variables:

```
int a = 0;
int lasta = a;

int b = 0;
int lastb = b;

int c = 0;
int lastc = c;
```

```
int button = LOW;
int buttonLast = LOW;

long buttonTime = 0;
long debounceDelay = 50;
```

- 9. In our loop() method, we are going to save the time we use sending a click event and check the button's state only if some time has passed. In our case, we have set the debounceDelay variable to 50 milliseconds:

```
void loop() {
  a = analogRead(0);
  if (abs( a - lasta ) > 10 ) {
    Serial.print("A");
    Serial.println( a );
    lasta = a;
  }
  b = analogRead(1);
  if (abs( b - lastb ) > 10 ) {
    Serial.print("B");
    Serial.println( b );
    lastb = b;
  }
  c = analogRead(2);
  if (abs( c - lastc ) > 10 ) {
    Serial.print("C");
    Serial.println( c );
    lastc = c;
  }
  if ( millis() > buttonTime + debounceDelay ) {
    button = digitalRead( 2 );
    if ( buttonLast == HIGH && button == LOW ) {
      Serial.println("click");
    }
    buttonLast = button;
    buttonTime = millis();
  }
}
```

- 10. Compile and upload the Arduino code and switch back to our Processing sketch.

- 11. Now start the Smilie-O-Mat sketch and change the facial parameters using the knobs of your controller and send some tweets using the button. Create a smiling face as shown in the following screenshot and post it on Twitter:

![](img/5cb67baa7bc7778de360b14c00d1ab81_142_0.png)

## Objective Complete - Mini Debriefing

The third task of our current mission was to connect our prototyped controller to the Smilie-O-Mat sketch and to extend the sketch to parse the messages from the serial port. In step 5, we added a serialEvent() callback method to get notified when messages arrive on the serial port, and we checked the first letter of the message to see which of the knobs have been turned. Then, we converted the rest of the string to a number and used it to control the facial parameters of our smiley.

Starting with step 7, we added support for the click messages our Arduino sketch is sending when the button was pressed. Since our button sometimes triggers more than one click message, we debounced the button by adding a little delay to the Arduino sketch so that our controller doesn’t fire the messages on accident.

## Putting it in a box

Now we have prototyped the controller, changed the Smilie-O-Mat sketch to support it, and tested it, but it is still a little bit brittle and hard to use. The current task is to create a customized controller that is easier and faster to use than a mouse, which we currently do not have.

What we need to do is put the controller into a more stable housing and add some knobs to the variable resistors. We will use an aluminum box to house our controller, drill some holes for the resistors, and then switch and solder the connections to prevent them from losing contact. We will also add a sheet of paper to prevent our Arduino from touching the metal directly and causing a short circuit.

Like the robots in Project 1, Romeo and Juliet, the version of the controller shown in this task is how I solved the problem, and this is by no means the only valid solution. You want to use a pink lunch box or a little plastic dinosaur instead of the aluminum box? No problem! This is your controller—feel free!

## Engage Thrusters

Let's build a housing for our controller:

- 1. Take off the lid of our box and mark the spots where you want your input elements to be.

- 2. Drill four holes; three for our variable resistors and one for the push button, as shown in the following picture:

![](img/5cb67baa7bc7778de360b14c00d1ab81_143_0.png)

- 3. Screw off the nut of your resistors, stick them through the holes you just made, and fix them with the nut again.
- 4. Turn the lid around and solder the wires to the middle connectors.
- 5. Now, prepare three pieces of wire and solder them to all the left legs of our resistors.
- 6. Solder the wire from the second resistor to the left leg of the first one, and the wire from the third resistor to the left leg of the second one.
- 7. Now, take three more pieces of wire and solder them to the right legs of the resistors.

- 8. Solder the wire from the second resistor to the right leg of the first one, and the wire from the third resistor to the right leg of the second one. Your circuit should look like the following picture:

![](img/5cb67baa7bc7778de360b14c00d1ab81_144_0.png)

- 9. Now unscrew the nut of your push button and place it in the remaining hole.

- 10. Fix it with the nut, and then solder the fixed resistor to one of the legs and also solder a wire to the second leg.

- 11. Now solder the remaining leg of the resistor to the right leg of the nearest variable resistors and solder the wire to the left leg.

- 12. We need to solder one final piece of wire to the leg of the push button where the resistor is soldered to. Your wiring should look as shown in the following diagram:

![](img/5cb67baa7bc7778de360b14c00d1ab81_144_1.png)

- 13. Take a metal cutter and cut an opening for the USB cable to the bottom half of our box. Make sure your gap is deep enough to support the cable, even when the lid is closed like as shown in this picture:

![](img/5cb67baa7bc7778de360b14c00d1ab81_145_0.png)

- 14. Place the Arduino in the box and connect the wire dangling from the left leg of the first resistor to the 5V pin of the Arduino board.

- 15. Now connect the wire from the right leg to the Gnd pin.

- 16. Hook the middle wires of the resistors to analog inputs 0, 1, and 2, and connect the wire we soldered to the push button to digital 2. The next picture shows how I connected my lid to the Arduino:

![](img/5cb67baa7bc7778de360b14c00d1ab81_145_1.png)

- 17. Close the lid of your box and stick some knobs to the turnable shaft of the variable resistors.

- 18. If you like you can label the knobs and the button using a sharpie pen. The next picture shows my finished controller board:

![](img/5cb67baa7bc7778de360b14c00d1ab81_146_0.png)

## Objective Complete - Mini Debriefing

In our final task for this mission, we removed the solderless breadboard from our circuit and soldered the components together. We didn't use a circuit board, but since our circuit is very simple, we directly connected the parts using a technique sometimes called **rat nest wiring**. We also created a housing for our controller by mounting the input controllers to a box.

As I already said in the introduction to this task, how your controller housing looks and how you lay out the input elements is mainly a matter of taste. Make this controller as personal or weird as you like.

## Mission Accomplished

Our current mission was to build a custom input controller for our Smilie-O-Mat. We started with a variable resistor and connected it to the Arduino—an AVR microcontroller development board. Arduino is programmed in C++, but the API is very similar to the Processing language, and the Arduino IDE uses the same framework as the Processing IDE. We created a program in the Arduino IDE that reads the value of our resistor and sends it to the computer via a serial port. On the Processing side, we used the serial library to read the messages and display them.

In the second task, *Building your controller*, we created a prototype of the controller by connecting three variable resistors and a button to a solderless breadboard. We created a simple protocol for the messages we send to the computer, which allowed us to send the value of the input and also which controller has changed. The button sends a **click** message to the computer every time the push button's state changes from pressed to released.

