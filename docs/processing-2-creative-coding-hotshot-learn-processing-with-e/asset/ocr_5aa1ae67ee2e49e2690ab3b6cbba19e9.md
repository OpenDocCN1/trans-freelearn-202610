13. So far, our dancer is getting tracked and drawn, but if you run the code now, it will float in a void. To change this, add the following code to the drawDancefloor() method to give our dancer something to dance on:

```
void drawFloor() {
  noStroke();
  fill( 128 );

  beginShape(QUADS);
  vertex( -400, -100, -400 );
  vertex( -400, -100, 400 );
  vertex( 400, -100, 400 );
  vertex( 400, -100, -400 );
  endShape();
}
```

14. We need to run our sketch now. Our stick figure will follow our dance moves, like in this screenshot:

![](bbox=[0.195, 0.422, 0.805, 0.78])