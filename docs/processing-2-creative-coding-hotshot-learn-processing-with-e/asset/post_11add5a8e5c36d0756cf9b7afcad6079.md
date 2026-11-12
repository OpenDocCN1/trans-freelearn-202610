
## Engage Thrusters

Let's tweet our mood:

- 1. Open the Smilie-O-Mat sketch from task 2, Let me change it.

- 2. The first thing we are going to add is a button to tweet the face, so let's reduce the size of the sliders to make room for it. Change the drawSliders() method like this:

```
void drawSliders() {
fill(0);
strokeWeight(1);
rect( 20,315,width-130,2 );
rect( 15 + map( mouth, 0, 1024, 0, width-130 ), 310, 10, 12 );

rect( 20,345,width-130,2 );
rect( 15 + map( eye, 0, 1024, 0, width-130 ), 340, 10, 12 );
rect( 20,375,width-130,2 );
rect( 15 + map( col, 0, 1024, 0, width-130 ), 370, 10, 12 );
}
```

- 3. We also need to adjust our mousePressed() and mouseDragged() methods to make the mapping of the mouse coordinates work with the shorter sliders.

```
void mousePressed() {
if ( mouseY > 310 && mouseY < 322 ) {
mclick = true;
mouth = constrain( int(
map( mouseX, 20, width - 110, 0, 1024 )), 0, 1024 );
}

if ( mouseY > 340 && mouseY < 352 ) {
eclick = true;
eye = constrain( int(
map( mouseX, 20, width - 110, 0, 1024 )), 0, 1024 );
}

if ( mouseY > 370 && mouseY < 382 ) {
cclick = true;
col = constrain( int(
map( mouseX, 20, width - 110, 0, 1024 )), 0, 1024 );
}
}
```

```
void mouseDragged() {
if (mclick) mouth = constrain(
int( map( mouseX, 20, width - 110, 0, 1024 )), 0, 1024 );
if (eclick) eye = constrain(
    int( map( mouseX, 20, width - 110, 0, 1024 ) ), 0, 1024 );
if (cclick) col = constrain(
    int( map( mouseX, 20, width - 110, 0, 1024 ) ), 0, 1024 );
}
```

- 4. Now add a drawButton() method and a Boolean variable that will indicate if the button is currently being pressed.

```
boolean bclick = false;
void drawButton() {
    fill( bclick ? 128 : 255);
    stroke( 0 );
    strokeWeight(2);
    rect( width - 90, 310, 70, 70 );
    fill(0);
    text( "tweet", width-83, 353 );
}
```

- 5. Call the drawButton() method from the draw() method to make our button show up.

```
void draw() {
    background(255);
    color c = color(map(col, 0, 1024, 0, 255), 255, 255 );
    fill(c);
    ellipse(150,150,250,250);
    strokeWeight(3);

    fill(255);
    ellipse( 120, 140, 60, 60);
    ellipse( 180, 140, 60, 60);
    fill(0);
    ellipse( 125, 150, 20, 20);
    ellipse( 175, 150, 20, 20);

    line( 100, 100 + map( eye, 0, 1024, -10, 10), 140, 100 - map( eye, 0, 1024, -10, 10) );
    line( 160, 100 - map( eye, 0, 1024, -10, 10), 200, 100 + map( eye, 0, 1024, -10, 10) );

    noFill();
    beginShape();
    curveVertex(100, 200);
    curveVertex(100, 200);
    curveVertex(150, 180 + map( mouth, 0, 1024, 0, 50) );
curveVertex(200, 200);
curveVertex(200, 200);
endShape();

drawSliders();
drawButton();
}
```

- 6. To make the button clickable, we need to handle the mouse events; so add the following code to our mousePressed() method:

```
void mousePressed() {
  if ( mouseY > 310 && mouseY < 380
    && mouseX > width - 90 && mouseX < width - 20 ) {
    bclick = true;
    return;
  }

  if ( mouseY > 310 && mouseY < 322 ) {
    mclick = true;
    mouth = constrain( int(
      map( mouseX, 20, width - 110, 0, 1024 )), 0, 1024 );
  }

  if ( mouseY > 340 && mouseY < 352 ) {
    eclick = true;
    eye = constrain( int(
      map( mouseX, 20, width - 110, 0, 1024 )), 0, 1024 );
  }

  if ( mouseY > 370 && mouseY < 382 ) {
    cclick = true;
    col = constrain( int(
      map( mouseX, 20, width - 110, 0, 1024 )), 0, 1024 );
  }
}
```

- 7. The click event is generated when the mouse button is released again. So, add the following code to the mouseReleased() method:

```
void mouseReleased() {
  if (bclick) {
    println( "button clicked" );
  }

  bclick = false;
  mclick = false;
  eclick = false;
  cclick = false;
}
```

- 8. Now drag the twitter4j-core.jar file and the keys.txt file that we generated in the last task for our Smilie-O-Mat.

- 9. We need to import the Twitter4J library; we also need an array of tweets to send. Add the import statement and the variable definitions to the beginning of the sketch.

```
import twitter4j.*;

Twitter twitter;
String[] tweets = {
    "I feel so #SmilieOMat",
    "I currently feel like this #SmilieOMat",
    "This is how I feel #SmilieOMat"
};
```

- 10. Add an initTwitter() method and copy the initialization code from our Twitter test application.

```
void initTwitter() {
String[] keys = loadStrings( "keys.txt" );

if (keys.length <= 2) {
try {
twitter = TwitterFactory.getSingleton();
twitter.setOAuthConsumer( keys[0], keys[1] );
RequestToken requestToken = twitter.getOAuthRequestToken();
AccessToken at = null;
if ( System.getProperty("os.name").toLowerCase().
indexOf("win")>=0) {
link(requestToken.getAuthorizationURL());
}
else {
open(requestToken.getAuthorizationURL());
}

String preset="";
String pin=javax.swing.JOptionPane.
showInputDialog(frame,"Enter your pin",preset);
at = twitter.getOAuthAccessToken(requestToken, pin);

String[] newKeys = new String[4];
newKeys[0] = keys[0];
newKeys[1] = keys[1];
newKeys[2] = at.getToken();
newKeys[3] = at.getTokenSecret();

saveStrings( "data/keys.txt", newKeys  );

} catch ( Exception e ) {
    e.printStackTrace();
}

} else {
ConfigurationBuilder cb = new ConfigurationBuilder();
cb.setOAuthConsumerKey( keys[0] );
cb.setOAuthConsumerSecret( keys[1] );
cb.setOAuthAccessToken( keys[2] );
cb.setOAuthAccessTokenSecret( keys[3] );

twitter = new TwitterFactory( cb.build() ).getInstance();
}
}
```

- 11. Now we call the initTwitter() method from our setup() method.

```
void setup() {
size(300,390);
smooth();
background(255);
colorMode(HSB);
textFont( createFont( "Georgia", 20 ) );

initTwitter();
}
```

- 12. To tweet the current smiley, we add a new method named tweet(). Since we only want to tweet the image of the smiley and not the GUI elements, we need to copy the section of the window we want to a PImage object and then save it to a file. We will use this file as a media object for our status updates to upload to Twitter.

```
void tweet() {

PImage image = get( 0, 0, 300,300 );
image.save( "smilie.png" );

java.io.File f = new java.io.File( sketchPath( "smilie.png" ) );
StatusUpdate status = new StatusUpdate( tweets[int(random(
tweets.length ))]);
status.setMedia(f);

try {
twitter.updateStatus(status);
}
catch ( Exception e ) {
    e.printStackTrace();
}
}
```

- 13. Now change the mouseReleased() method to make it call the new tweet() method instead of making it print a message.

```
void mouseReleased() {
if ( bclick ) {
    tweet();
}
bclick = false;
mclick = false;
eclick = false;
cclick = false;
}
```

- 14. Run your sketch, create a smiling happy smiley, and tweet it using our tweet button. In the following screenshot, you can see our Smilie-O-Mat and the resulting tweet:

![](img/5cb67baa7bc7778de360b14c00d1ab81_120_0.png)

## Objective Complete - Mini Debriefing

In task 4, we combined all the sketches we have written for this mission and added the Twitter functionality to our Smilie-O-Mat. We added a button to trigger the tweeting and added the key setup mechanism we implemented in task 3.

We also learned how to add a media file to our Twitter status update. In this sketch, the media file was the image of the smiley that we created.

## Mission Accomplished

Our mission was to implement a sketch that makes it easier to communicate the current emotional status of a user. We have accomplished this by creating a smiley that has various controllable facial parameters. In task 1, we implemented the drawing routine that generates our smiley, but changing the parameters meant changing a variable and running the sketch again.

To tackle this problem, we added some GUI elements to our sketch in task 2 to simplify the interaction with our sketch for the user. We didn’t use any existing GUI libraries, but instead created the sliders from scratch by implementing the mouse event callback functions and drawing some rectangles.

In task 3, we created the necessary code to authenticate our app with twitter.com and created a method that allows our app to post status updates for a Twitter user. We created a consumer key and a consumer secret key on the Twitter developer site and asked the user for permission to tweet using a request URL and pin code.

And finally, we combined the two sketches in task 4 by adding a button to our GUI that tweets a status message on Twitter and adds the face the user has adjusted. We created an image containing the smiley by sparing the GUI elements and copying only a section of the sketch window to a new PImage object; we then saved this file and added it as a media object to the status update.

## You Ready to go Gung HO? A Hotshot Challenge

The Smilie-O-Mat is finished, but this doesn’t mean you have to stop here. Why don’t you try to take it to the next level and implement one of the following suggestions:

- - Parse the Twitter stream for the #SmilieOMat messages of people you follow
- Make additional facial features changeable
- Create a mobile version
- Publish your smiley on other social networks
