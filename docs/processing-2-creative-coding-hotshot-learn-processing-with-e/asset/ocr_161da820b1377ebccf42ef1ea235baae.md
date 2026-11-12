```
if ( "J".equals( l.speaker ) ) {
  juliet.speak( l.text );
}
if ( "R".equals( l.speaker ) ) {
  romeo.speak( l.text );
}
}
running = false;
}
```

+   5. Since each of our robots gets his own speaker, we don't want to hear the text on both speakers. We want one robot to use the right one and the other robot, the left one. Fortunately, ttslib provides a speakLeft() and a speakRight() method, which do exactly what we need. So change the two speak() methods to look like the following:

```
...
if ( "J".equals( l.speaker ) ) {
  juliet.speakLeft( l.text );
}
if ( "R".equals( l.speaker ) ) {
  romeo.speakRight( l.text );
}
...
```

+   6. Currently, our draw() method is somewhat boring and also somewhat misleading if the drama thread is already running. So we will change it to display five lines of the currently read script. Add an if statement to the draw() method that checks the state of the running variable we added to our Drama thread earlier.

```
void draw() {
  background(255);
  textAlign(CENTER);
  fill(0);

  if ( !drama.isRunning() ) {
    text( "Click here for Drama", width/2, height/2 );
  } else {
  }
}
```