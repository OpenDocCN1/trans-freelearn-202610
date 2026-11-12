
## Objective Complete - Mini Debriefing

For the first task of our current mission, we created a Processing sketch that draws our basic face. We added three variables that control the facial features we are enabling the user to change. We changed the color mode to HSB to make it easier for the user to select the desired color tone. Like RGB, the HSB color mode has three parameters, but instead of mixing red, green, and blue, you choose the basic color (hue), the brightness, and the saturation. We keep the brightness and saturation value fixed for the sake of simplicity and let the user change the hue value.

All the control variables store a value between 0 and 1023, which are then mapped to the value range we need for drawing using the map() command.

## Let me change it

Nobody feels like smiling all the time, but it is currently somewhat cumbersome to change the facial parameters of our smiley, which could have an impact on your current state of emotion. To prevent our Smillie-O-Mat from increasing the user's level of frustration or annoyance, our second task for this mission is to make the facial parameters adjustable using the mouse by adding three sliders below the smiley.

## Engage Thrusters

Let's now change our face:

- 1. Open the Smillie-O-Mat sketch and change the size of our sketch window in the `setup()` method to add more room for the sliders below the smiley face.

```java
void setup() {
  size(300, 390);
  smooth();
  background(255);
  colorMode(HSB);
}
```

- 2. Now add a new method named `drawSliders()` and draw three black rectangles for the bases of our sliders.

```java
void drawSliders() {
  fill(0);
  strokeWeight(1);
  rect( 20,315,width-40,2 ) ;
  rect( 20,345,width-40,2 ) ;
  rect( 20,375,width-40,2 ) ;
}
```

- 3. The first slider will change the mouth position, so we add a smaller rectangle and map the value of the mouth variable to the x-coordinate of our rectangle.

```
void drawSliders() {
    fill(0);
    strokeWeight(1);
    rect( 20,315,width-40,2 );
    rect( 15 + map( mouth, 0, 1024, 0, width-40 ), 310, 10, 12 );
    rect( 20,345,width-40,2 );
    rect( 20,375,width-40,2 );
}
```

- 4. Add a slider for the eyebrow angle and one for the color of our face, the same way you did for the mouth variable.

```
void drawSliders() {
    fill(0);
    strokeWeight(1);
    rect( 20,315,width-40,2 );
    rect( 15 + map( mouth, 0, 1024, 0, width-40 ), 310, 10, 12 );
    rect( 20,345,width-40,2 );
    rect( 15 + map( eye, 0, 1024, 0, width-40 ), 340, 10, 12 );
    rect( 20,375,width-40,2 );
    rect( 15 + map( col, 0, 1024, 0, width-40 ), 370, 10, 12 );
}
```

- 5. Now we call our drawSliders() method from the draw() method.

```
void draw() {
    background(255);
    drawSliders();
    color c = color(map(col, 0, 1024, 0, 255), 255, 255 );
    fill(c);
    ellipse(150,150,250,250);
    strokeWeight(3);
}
```

- 6. Run your sketch, then make changes to the control variables and check whether the position of the slider changes.

- 7. To make the slider react to the mouse, we add a new method named `mousePressed()` and a Boolean variable named `mclick`. This method gets called every time the mouse button is pressed; we will now test whether the mouse click occurred on our mouth slider.

```
boolean mclick = false;
void mousePressed() {
 if ( mouseY > 310 && mouseY < 322 ) {
 mclick = true;
 mouth = constrain( int( map( mouseX, 20, width - 20, 0, 1024
 ) ), 0, 1024 );
 }
}
```

- 8. We need to add another callback method that gets called when the user drags the mouse. We change the position of the slider only if the `mclick` variable is set to `true` by our `mousePressed()` method.

```
void mouseDragged() {
 if ( mclick ) mouth = constrain( int( map( mouseX, 20, width - 20,
 0, 1024 ) ), 0, 1024 );
}
```

- 9. We set the `mclick` variable to `true` when the slider is clicked on. When the mouse button is released, we need to set the variable back to `false`. Add a `mouseReleased()` method to our sketch.

```
void mouseReleased() {
 mclick = false;
}
```

- 10. Run the sketch, drag the first slider, and check whether the position of the mouth changes accordingly.

- 11. To make our eye and color sliders work, we need two more Boolean variables, and we need to add the following code to our `mousePressed()` method:

```
boolean mclick = false;
boolean eclick = false;
boolean cclick = false;

void mousePressed() {
 if ( mouseY > 310 && mouseY < 322 ) {
 mclick = true;
 mouth = constrain( int( map( mouseX, 20, width - 20, 0, 1024
 ) ), 0, 1024 );
 }

 if ( mouseY > 340 && mouseY < 352 ) {
 eclick = true;
 eye = constrain( int( map( mouseX, 20, width - 20, 0, 1024 ) ),
 0, 1024 );
 }

 if ( mouseY > 370 && mouseY < 382 ) {
 cclick = true;
 col = constrain( int( map( mouseX, 20, width - 20, 0, 1024 ) ),
 0, 1024 );
 }
}
```

- 12. We also need to handle the dragging and releasing of the mouse button by adding the following code to the mouseDragged() and mouseReleased() methods:

```
void mouseDragged() {
if ( mclick ) mouth = constrain( int( map( mouseX, 20, width - 20,
0, 1024 ) ), 0, 1024 );
if ( eclick ) eye = constrain( int( map( mouseX, 20, width - 20,
0, 1024 ) ), 0, 1024 );
if ( cclick ) col = constrain( int( map( mouseX, 20, width - 20,
0, 1024 ) ), 0, 1024 );
}
```

```
void mouseReleased() {
mclick = false;
eclick = false;
cclick = false;
}
```

- 13. Now run our sketch and use the sliders to adjust the facial parameters to match your current mood. In this screenshot, you can see one of the smileys that can be generated:

![](img/5cb67baa7bc7778de360b14c00d1ab81_105_0.png)

## Objective Complete - Mini Debriefing

For this task, we added three sliders that change the values of our control variables. We didn’t use any existing GUI libraries; we created the sliders from scratch by mapping the values of our variables that fall within the interval 0 to 1024 to the length of the slider and drawing two rectangles.

We also implemented three callback functions to make the sliders react to mouse events. In the mousePressed() method, we test whether the mouse pointer is on one of our sliders, and if it is, we set the Boolean variable for this slider to true. In the mouseDragged() method, we change the value of our control variables if the Boolean variable for this parameter is true. When the mouse button is released, we set all Boolean variables to false in the mouseReleased() method.

## Hello Twitter

Task 3 of our mission is to create a Processing sketch that is able to change our current Twitter status. We will use the Java library Twitter4J from twitter4j.org, which encapsulates all the Twitter API calls into convenient Java objects to set the status. We will also use the Twitter developer website to create the necessary application keys and learn how to ask the user for permission to post a status update.

## Prepare for Lift Off

To complete this and the following task of our current mission, you will need a Twitter account to post the tweets and access to the Twitter developer site. If you are already using Twitter, you are fulfilling the prerequisites for this task, as Twitter doesn’t distinguish between normal and developer accounts; every Twitter user has the permission to create and register applications. If you have no Twitter account yet, go to twitter.com and register for one.

## Engage Thrusters

Let's tweet a bit:

- 1. Go to the Twitter developer pages at https://dev.twitter.com/ and log in with your Twitter account.

- 2. Now click on your account and select My applications from the pop-up menu in the upper-right corner, as can be seen in the following screenshot:

![](img/5cb67baa7bc7778de360b14c00d1ab81_107_0.png)

- 3. Now register a new application by clicking on **Create a new Application** and filling out the registration form.

- 4. Switch to the **Settings** tab and change the **Application Type** to **Read and Write** to allow our app to post tweets.

- 5. Now switch back to the **Details** tab and create the consumer key in **Consumer key** and the **Consumer Secret** key by clicking on **Recreate my access token**. The following screenshot shows the **Details** page for my Smilie-O-Mat application:

![](img/5cb67baa7bc7778de360b14c00d1ab81_107_1.png)
