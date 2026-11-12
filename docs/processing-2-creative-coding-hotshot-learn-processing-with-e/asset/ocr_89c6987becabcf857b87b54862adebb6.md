```
moon = new int[width/10+1];
for ( int i=0; i < moon.length; i++ ) {
  moon[i] = int(random(10));
}
landingX = int( random(3, moon.length-4) )*10;

pos = new PVector( 150, 20 );
a = 0;
acc = 0;
speed = new PVector(0,0);
}
```

+   13. In our draw() method, we will check the state variable and only call the update() method when the state is RUNNING. We will also add the drawWaiting() and drawFinished() methods that get called in the corresponding states.

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
  drawLandingZone();
  drawShip();

  if ( state == WAITING ) {
    drawWaiting();
  }
  else if ( state == RUNNING ) {
    update();
  }
  else if ( state == FINISHED ) {
    drawFinished();
  }
}
```

```
void drawWaiting() {
  textAlign( CENTER );
  fill(0);
}
```