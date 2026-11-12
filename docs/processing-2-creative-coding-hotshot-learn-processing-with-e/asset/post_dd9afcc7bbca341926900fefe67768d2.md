
In the third task, Changing your faces, we extended the Smile-O-Mat sketch we created in Project 4 to parse the messages from our controller. We implemented a serialEvent() callback method that gets called every time a message arrives at the serial port. In this method, we checked the first letter of the message, and if it starts with A, B, or C, we parsed the rest of the string to an integer. These integers are used to control the facial parameters of our smiley. If the message is click, then we triggered the tweet() method, which sends the current smiley to the tweeter.

When a button is pressed or released, there is a short period of time where the contacts are very near to each other but not fully closed. In this time, short contacts of the button can trigger multiple click events accidentally. To prevent this from happening, we created a 50 millisecond time slot where we don't send a second click message. This technique is called debouncing, and is very common when dealing with switches or buttons.

In the final task, Putting it in box, we created a housing for the controller and made the connections between our components permanent by soldering them together. We didn't use a circuit board, but soldered the components together directly. We used an aluminum box for housing and laid out the knobs in a straight line, but the layout of the elements and the material of the box are a matter of taste. This controller should reflect your workflow and your personal preferences. Make it as traditional or as crazy as you like it.

## You Ready to go Gung HO? A Hotshot Challenge

Arduino is an incredibly cool platform for electronic prototypes for beginners and professionals alike. In this project, we have certainly only scratched the surface of its possibilities. If you like tinkering with electronics, try to extend your Smile-O-Mat sketch in one of the following ways:

- Add additional knobs for more facial parameters
- Add a potentiometer or a rotary encoder to choose the text message to tweet
- Add an LCD display to show the last tweet
- Connect some servos and RGB LEDs to Arduino and create an animatronics' version of the face

## Project 6

### Fly to the Moon

Ever since people got their hands on computers, they have used them for playing games. One of the first games was a simulation game named Lunar Lander, where the player had to land a lunar-landing capsule on the moon. The first version was written in 1969 and was text-based. The first graphical version of this game was written four years later for DEC Terminals. The moon-landing simulation games have been ported to nearly every computer system ever since, which makes it a perfect game for our current mission, since we are going to use the Processing modes to make a game run in the browser using Processing.js and on Android devices.

### Mission Briefing

Our current mission is to create a moon-lander game. The first version of the game will be a Processing sketch running on your computer like the ones we have created in the previous missions. Then, we will use the various Processing modes and make the game run in the browser using the JavaScript mode, and on Android devices using the Android mode.

For each of these modes, we will adjust the game controls to fit the platform if necessary.

### Why Is It Awesome?

This project is awesome for two reasons: first, we are writing a game, and this has been on the official list of computer project awesomeness since the invention of computers, and second, we write a game that runs on your computer, your Android-powered mobile device, and in your browser. This allows us to cover nearly every Internet-capable platform that currently exists, from desktop computers and laptops to mobile phones, tablets, and smart TV's. When Java arrived, it made the promise of write once, run everywhere. Processing is taking this promise to the next level.

### Your Hotshot Objectives

This mission is split into four tasks. We will write a moon-lander simulation game in the first two tasks, and make it run in the browser and on Android devices in the remaining two tasks. The following is the list of objectives for our current task:

- Drawing a sprite
- Initiating the landing sequence
- Running your sketch in the browser
- Running the game on an Android phone

### Mission Checklist

The first two tasks of this mission are implemented using only Processing. In the third task, you will need a browser that supports the canvas element, like Firefox, Chrome, Safari, or Internet Explorer starting with Version 8.

For the last task, we will need to download and install the Android SDK, which is described in detail later.

### Drawing a sprite

The first task for our current mission is to create a sprite showing a spaceship and make it move and rotate on the screen. We will make use of the translate() and rotate() methods that Processing provides. We will also generate the level design for our game and generate a landing platform for our spaceship.

To make the game more interesting, these levels will be recreated every time the game is restarted.

### Engage Thrusters

Let's start creating the level design:

1. Create a new Processing sketch and add the setup() and draw() methods, as shown in the following code snippet:

```
void setup() {
}

void draw() {
}
```

2. Now we set the size of the window to 300 by 300 and define an array of integers, which we will use to draw our moon.

```
int[] moon;

void setup() {
  size( 300, 300 );
  moon = new int[width/10+1];
  for ( int i=0; i < moon.length; i++ ) {
    moon[i] = int( random( 10 ) );
  }
}
```

3. In our draw() method, we add a light blue grid that fills a white page to make it look like we made our level on a page from our mathematics notebook.

```
void draw() {
  background(255);
  stroke(200, 200, 255);
  for ( int i=0; i<height/10; i++ ) {
    line( 0, i*10, width, i*10 );
  }

  for ( int i=0; i<width/10; i++ ) {
    line( i*10, 0, i*10, height );
  }
}
```

4. To draw our moon, we iterate over the array we initialized in the setup() method and fill it with a transparent yellow color. We will add a drawMoon() method to our draw() method and create our yellow polygon using beginShape() and endShape().

```
void draw() {
  background(255);
  stroke(200, 200, 255);
  for ( int i=0; i<height/10; i++ ) {
    line( 0, i*10, width, i*10 );
  }

  for ( int i=0; i<width/10; i++ ) {
    line( i*10, 0, i*10, height );
  }

drawMoon();

}

void drawMoon() {
stroke(0);
fill(255, 200, 0, 60);
beginShape();
vertex(0, height);
for ( int i=0; i < moon.length; i++) {
vertex( i * 10, height - 20 - moon[i] );
}
vertex(width, height);
endShape(CLOSE);
}
```

5. Run your sketch; it should look like the following screenshot:

![](img/5cb67baa7bc7778de360b14c00d1ab81_152_0.png)

6. In this game, the player should land a rocket on a landing platform. We are going to align the platform to the grid we created in step 3 to make it look like we colored some of the boxes of our grid. Add a variable to store the landing platform's x coordinate and initialize it in the setup() method.

```
int[] moon;
int landingX = 0;

void setup() {
size( 300, 300 );
moon = new int[width/10+1];
for ( int i=0; i < moon.length; i++) {
moon[i] = int( random( 10 ));
}
landingX = int( random(3, moon.length-4) )*10;
}
```

7. Now, we add a `drawLandingZone()` method to our `draw()` method and implement it by drawing a rectangle and two lines for the feet. We will use the moon array to calculate where our feet will touch the ground.

```
void draw() {
background(255);
stroke(200, 200, 255);
for ( int i=0; i<height/10; i++) {
line( 0, i*10, width, i*10);
}
for ( int i=0; i<width/10; i++) {
line( i*10, 0, i*10, height );
}
drawMoon();
drawLandingZone();
}
```

```
void drawLandingZone() {
fill(128, 200);
rect( landingX - 30, height - 50, 60, 10);
line( landingX - 30, height - 20 - moon[landingX/10-3], landingX
- 20, height - 40 );
line( landingX + 30, height - 20 - moon[landingX/10 +3],
landingX + 20, height - 40 );
}
```

8. We have our moon and a landing zone, but we still don't have a ship to land. So, draw an image of a rocket or download the image from the book's support material and add it to your sketch by dropping it on the sketch window or by using the Sketch | Add File ... menu.

9. Now we will add a PImage variable and load the image in our setup() method.

```
int[] moon;
int landingX = 0;
PImage ship;
void setup() {
size( 300, 300 );
moon = new int[width/10+1];
for ( int i=0; i < moon.length; i++) {
moon[i] = int(random(10));
}
landingX = int( random(3, moon.length-4) )*10;
ship = loadImage( "ship.png" );
}
```
