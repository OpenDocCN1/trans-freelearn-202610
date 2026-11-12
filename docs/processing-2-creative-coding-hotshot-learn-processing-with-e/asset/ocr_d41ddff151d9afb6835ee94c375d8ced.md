In the third task, Changing your faces, we extended the Smile-O-Mat sketch we created in Project 4 to parse the messages from our controller. We implemented a serialEvent() callback method that gets called every time a message arrives at the serial port. In this method, we checked the first letter of the message, and if it starts with A, B, or C, we parsed the rest of the string to an integer. These integers are used to control the facial parameters of our smiley. If the message is click, then we triggered the tweet() method, which sends the current smiley to the tweeter.

When a button is pressed or released, there is a short period of time where the contacts are very near to each other but not fully closed. In this time, short contacts of the button can trigger multiple click events accidentally. To prevent this from happening, we created a 50 millisecond time slot where we don't send a second click message. This technique is called debouncing, and is very common when dealing with switches or buttons.

In the final task, Putting it in box, we created a housing for the controller and made the connections between our components permanent by soldering them together. We didn't use a circuit board, but soldered the components together directly. We used an aluminum box for housing and laid out the knobs in a straight line, but the layout of the elements and the material of the box are a matter of taste. This controller should reflect your workflow and your personal preferences. Make it as traditional or as crazy as you like it.

# You Ready to go Gung HO? A Hotshot Challenge

Arduino is an incredibly cool platform for electronic prototypes for beginners and professionals alike. In this project, we have certainly only scratched the surface of its possibilities. If you like tinkering with electronics, try to extend your Smile-O-Mat sketch in one of the following ways:

+   - Add additional knobs for more facial parameters
+   - Add a potentiometer or a rotary encoder to choose the text message to tweet
+   - Add an LCD display to show the last tweet
+   - Connect some servos and RGB LEDs to Arduino and create an animatronics' version of the face