
vertx[h][a] = sin( radians( a*5.0 ) ) * r;
}
}
}

- 4. Now we can replace our shape with a stack of triangle strips in our `draw()` method. We need to enable the depth test before we start drawing our shape and disable it afterwards to make sure that the GUI elements are drawn correctly. We will also enable some light sources to prevent our object from being shaded completely flat.

```
void draw() {
  hint( ENABLE_DEPTH_TEST );
  pushMatrix();
  background(255);
  fill(200);
  noStroke();

  translate( width/2, height/2);
  scale( 1.5 );
  ambientLight( 50, 50, 50);
  directionalLight( 100, 95, 55, 0, 0, -10 );
  pointLight( 150, 150, 200, 100, -100, 200 );

  translate(0, -50, 0);
  initPoints();

  beginShape(TRIANGLE_STRIP );
  for ( int h = 1; h < 20; h++) {
    for ( int a = 0; a<73; a++ ) {
      int aa = a % 72;
      normal( vertx[h][aa], 0, verty[h][aa] );
      vertex( vertx[h][aa], h*5.0, verty[h][aa] );
      normal( vertx[h-1][aa], 0, verty[h-1][aa] );
      vertex( vertx[h-1][aa], (h-1)*5.0, verty[h-1][aa] );
    }
  }
  endShape();

  popMatrix();
  hint(DISABLE_DEPTH_TEST );
}
```

- 5. Currently, our object is only visible from the side and can't be turned. To fix this, we need to calculate an axis of rotation and a rotational angle. To simplify these calculations, we need to add a class named Quaternion. Quaternions are mathematical constructs similar to complex numbers, only they are four-dimensional. Click on the New Tab menu to the right and enter the name as Quaternion. Then, add the following code:

```
class Quaternion {
  float w = 1.0;
  float x = 0.0;
  float y = 0.0;
  float z = 0.0;

  void mult( Quaternion b ) {
    Quaternion res = new Quaternion();
    res.w = this.w * b.w - this.x * b.x - this.y * b.y - this.z * b.z;
    res.x = this.w * b.x + this.x * b.w + this.y * b.z - this.z * b.y;
    res.y = this.w * b.y - this.x * b.z + this.y * b.w + this.z * b.x;
    res.z = this.w * b.z + this.x * b.y - this.y * b.x + this.z * b.w;
    set( res );
  }

  void set( PVector cross, float dot ) {
    this.x = cross.x;
    this.y = cross.y;
    this.z = cross.z;
    this.w = dot;
  }

  void set( Quaternion in ) {
    this.w = in.w;
    this.x = in.x;
    this.y = in.y;
    this.z = in.z;
  }

  float[] matrix() {
    float[] res = new float[4];
    float sa = (float) Math.sqrt(1.0f - w * w);

    if (sa < EPSILON) { sa = 1.0f; }

    res[0] = (float) Math.acos(w) * 2.0f;
    res[1] = x / sa;
    res[2] = y / sa;
    res[3] = z / sa;

    return res;
  }
}
```

- 6. We need to define three `Quaternion` objects in our main tab, which we will use to calculate the current rotation matrix when the user is dragging the mouse. We also need a `radius` variable and two `PVector` objects to track the current mouse position and state.

```
import controlP5.*;

ControlGroup box;
ControlP5 cp5;
Button toggleButton;

Quaternion qdown = new Quaternion();
Quaternion qnow = new Quaternion();
Quaternion qdrag = new Quaternion();

float radius = 200;
PVector down = new PVector();
PVector drag = new PVector();

float[][] vertx;
float[][] verty;
```

- 7. Now we need to add the `mousePressed()` and `mouseDragged()` methods, which change our variables when the mouse moves. We also add a helper function named `mouseToSphere()`, which is used to convert the coordinates of our mouse pointer to a point on a virtual sphere around our object, to calculate our quaternions.

```
PVector mouseToSphere(float x, float y) {
  PVector v = new PVector();
  v.x = (x - width/2) / radius;
  v.y = (y - height/2) / radius;

  float mag = v.x * v.x + v.y * v.y;
  if (mag > 1.0f) {
    v.normalize();
  } else {
    v.z = sqrt( 1.0f - mag );
  }
  return v;
}
```

```
void mousePressed() {
  if ( !box.isVisible() || ( mouseX > box.getWidth()
    || mouseY > box.getBackgroundHeight() ) ) {
    down = mouseToSphere( mouseX, mouseY );
    qdown.set( qnow );
    qdrag = new Quaternion();
  }
}
```

```
void mouseDragged() {
  if ( !box.isVisible() || ( mouseX > box.getWidth()
    || mouseY > box.getBackgroundHeight() ) ) {
    drag = mouseToSphere( mouseX, mouseY );
    qdrag.set( down.cross( drag ), down.dot( drag ) );
  }
}
```

## 8. To make use of our rotation matrix, we need to add the following code to our draw() method:

```
void draw() {
  hint( ENABLE_DEPTH_TEST );
  pushMatrix();
  background(255);
  fill(200);
  noStroke();

  translate( width/2, height/2);
  scale( 1.5 );
  ambientLight( 50, 50, 50 );
  directionalLight( 100, 95, 55, 0, 0, -10 );
  pointLight( 150, 150, 200, 100, -100, 200 );

  qnow.set( qdrag );
  qnow.mult( qdown );

  float[] matrix = qnow.matrix();
  rotate( matrix[0], matrix[1], matrix[2], matrix[3] );
  translate(0, -50, 0);
  initPoints();
}
```

- 9. Now we can rotate our object by dragging the mouse, but it's hard to distinguish between the top and bottom of our object after a few rotations. So we're going to close the bottom by adding a triangular fan to our draw() method.

```
void draw() {
  beginShape(TRIANGLE_STRIP);
  for ( int h = 1; h < 20; h++) {
    for ( int a = 0; a<73; a++ ) {
      int aa = a % 72;
      normal( vertx[h][aa], 0, verty[h][aa]);
      vertex( vertx[h][aa], h*5.0, verty[h][aa] );
      normal( vertx[h-1][aa], 0, verty[h-1][aa] );
      vertex( vertx[h-1][aa], (h-1)*5.0, verty[h-1][aa] );
    }
  }
  endShape();
  beginShape(TRIANGLE_FAN);
  int h = 19;
  vertex( 0, h*5, 0 );
  for ( int a = 0; a<73; a++ ) {
    int aa = a % 72;
    vertex( vertx[h][aa], h*5, verty[h][aa] );
  }
  endShape();
  popMatrix();
  hint(DISABLE_DEPTH_TEST);
}
```

- 10. So far, our object is a cylindrical extrusion of our shape from the first task. Currently, we're only using the angle as an input parameter for our getR() function. Now let's add the height as an additional input and create some new control variables.

```
float getR( float a, float h ) {
  float r = v2 * sin( radians(a) * v1 + ( h/15 )*v3 ) +
    sin(radians(3.6*h) * v4 + v5)*v6 + 40;
  return r;
}
```

```
void initPoints() {
    for ( int h = 0; h < 20; h++) {
        for ( int a = 0; a<72; a++) {
            float r = getR( a*5.0, h*5.0 );
            vertx[h][a] = cos( radians( a*5.0 ) ) * r;
            verty[h][a] = sin( radians( a*5.0 ) ) * r;
        }
    }
}
```

- 11. In our `setupGUI()` method, we need to add four more sliders to enter the new control values. To simplify the creation of the sliders, we will add a method named `addSlider()`.

```
float v1;
float v2;
float v3;
float v4;
float v5;
float v6;
```

```
void setupGUI() {
    cp5 = new ControlP5(this);
    toggleButton = cp5.addButton("toggleBox", 1, 0, 0, 100, 20);
    toggleButton.setLabel("Hide Menu");

    box = cp5.addGroup( "box", 0, 0, 150);
    box.setBackgroundHeight(170);
    box.setBackgroundColor(color(0, 100));

    Slider slider = addSlider( "slider", 40, 1, 8, 5, "count" );
    slider.setNumberOfTickMarks(8);
    Slider slider2 = addSlider( "slider2", 60, -10, 10, 5, "radius" );
    Slider slider3 = addSlider( "slider3", 80, -2, 2, 2, "twist" );
    Slider slider4 = addSlider( "slider4", 100, 0, 2, 1.5, "hcount" );
    Slider slider5 = addSlider( "slider5", 120, -2, 2, 2, "phase" );
    Slider slider6 = addSlider( "slider6", 140, 0, 5, 5, "hradius" );
}
```

```
Slider addSlider( String callback, int pos, float start, float end, float value, String label ) {
  Slider slider = cp5.addSlider(callback);
  slider.setPosition(10, pos);
  slider.setSize(80, 10);
  slider.setRange(start, end);
  slider.setValue(value);
  slider.setLabel( label );
  slider.moveTo( box );
  return slider;
}
```

- 12. We also need a new callback variable for each one of our sliders.

```
void slider( float val ) { v1 = val; }
void slider2( float val ) { v2 = val;}
void slider3( float val ) { v3 = val; }
void slider4( float val ) { v4 = val; }
void slider5( float val ) { v5 = val; }
void slider6( float val ) { v6 = val;}
```

- 13. Run your sketch now and change the sliders to create shapes like the one in this screenshot:

![](img/5cb67baa7bc7778de360b14c00d1ab81_243_0.png)

## Objective Complete - Mini Debriefing

For this task of our current mission, we extruded the 2D shape from the first task and created a cylindrical form from it. We added a Quaternion class and two callback functions that receive mouse press and mouse drag events, and calculated a rotation matrix that allows the user of our sketch to view the object from all sides.
