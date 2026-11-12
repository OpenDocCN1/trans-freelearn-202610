
## Your Hotshot Objectives

Our current mission objectives are:

- Rotating a sphere
- Let there be light
- From sphere to globe
- From globe to neon globe

## Mission Checklist

For this mission, we need no libraries, SDKs, or any additional software apart from Processing. We are going to create 3D graphics, so you do need a graphics card in your computer that is capable of accelerating 3D renderings and which supports OpenGL 2.0 or later versions. If your computer or graphics card was built after 2005, you are most certainly fine.

## Rotating a sphere

The first task of our current mission is to create the mesh of a sphere and make it rotate on the screen. We will create a mesh resembling the latitude and longitude rings found on a globe. To simplify the mathematics required to create these rings, we will use polar coordinates and convert them to Cartesian XYZ coordinates when creating our sphere. Processing can make use of your graphics card’s memory when storing the model data, which improves the speed of drawing the object on the screen a lot if the object itself is static. Since our sphere doesn't change while our sketch is running, we will use a PShape object to store the model data.

## Engage Thrusters

Let's start with the creation of our sphere's mesh:

- 1. Open a new sketch and add the `setup()` and `draw()` methods.

```
void setup() {
}

void draw() {
}
```

- 2. In the `setup()` method, we define our sketch window size and set the rendering mode to P3D. We also create a `PShape` object named `sphere` to store the mesh data. `PShape sphere;`

```
void setup() {
  size(400, 400, P3D);
}
```

- 3. Next, we create a new method named `makeSphere()`, which takes the radius and step size (in degrees) as parameters. The return value of our function is the new `PShape` object containing our sphere's vertices.

`PShape makeSphere( int r, int step ) {`

- 4. Polar coordinates use two angles and a radius to define a point on a spherical surface; every point has the same distance to the origin, so our radius stays fixed, but our angles change. We will now define two nested loops that iterate over all the possible angle positions.

```
PShape makeSphere( int r, int step ) {
  PShape s = createShape();

  s.noFill();
  s.stroke(255);
  s.strokeWeight(2);

  for (int i = 0; i < 180; i += step) {
    for (int j = 0; j <= 360; j += step) {
    }
  }

  return s;
}
```

- 5. Now we will be using the polar coordinates to generate rings on our sphere using quad strips. We need to define point pairs (like in the following figure) and use the `vertex()` method to feed the converted Cartesian coordinates to our `PShape` object:

![img/5cb67baa7bc7778de360b14c00d1ab81_177_0.png]

- 6. To convert the polar coordinates into Cartesian ones, we need to calculate the sine and cosine values of the angles. Processing expects the angles (which we use as an input for trigonometric functions or rotate statements) to be in radians, which go from 0 to 2π instead of 0 to 360 for a full circle. We will use the radians() method to convert the angles before feeding them to those functions.

```
PShape makeSphere( int R, int step ) {
  PShape s = createShape();
  s.beginShape(QUAD_STRIP);
  s.noFill();
  s.stroke(255);
  s.strokeWeight(2);
  for (int i = 0; i < 180; i += step) {
    float sini = sin(radians(i));
    float cosi = cos(radians(i));
    float sinip = sin(radians(i + step));
    float cosip = cos(radians(i + step));
    for (int j = 0; j <= 360; j += step) {
      float sinj = sin(radians(j));
      float cosj = cos(radians(j));
      s.vertex(R * cosj * sini, R * -cosi, R * sinj * sini);
      s.vertex(R * cosj * sinip, R * -cosip, R * sinj * sinip);
    }
  }
  s.endShape();
  return s;
}
```

- 7. In our `setup()` method, we add a call to our `makeSphere()` method.

```
void setup() {
  size(400, 400, P3D);
  sphere = makeSphere(150, 5);
}
```

- 8. To make the sphere visible, we need to make use of our sphere object in the `draw()` method by using the `shape()` method.

```
void draw() {
  background(0);
  translate(width/2, height/2);
  shape(sphere);
}
```

- 9. Run your sketch now. The sphere should look like this:

![img/5cb67baa7bc7778de360b14c00d1ab81_179_0.png]

- 10. Now we are going to make our sphere rotate. In our `setup()` method, we fix the frame rate to 25 frames per second using the `frameRate()` method.

```
void setup() {
  size(400, 400, P3D);
  frameRate(25);
  sphere = makeSphere(150, 5);
}
```

- 11. Next, we define a variable for the angle of rotation and increase it with every frame in our `draw()` method. We also add a call to `rotateX()` to tilt the sphere slightly, and a call to `rotateY()` to make it rotate around the y axis.

```
float a = 0;

void draw() {
  background(0);
  translate(width/2, height/2);
  pushMatrix();
  rotateX(radians(-30));
  rotateY(a);
  a += 0.01;
  shape(sphere);
  popMatrix();
}
```

- 12. Run your sketch. We now have a rotating wireframe model of a sphere.

## Objective Complete - Mini Debriefing

For our first task of this mission, we created the 3D mesh of a sphere. In steps 1 to 5, we created a method named `makeSphere()`, which uses two `for` loops to generate points on a spherical surface in polar coordinates. We then converted the points to Cartesian coordinates and generated quad strips to form our mesh. We used a `PShape` object to store our mesh data, as the mesh data won't change at runtime and Processing needs to transfer the vertex data to the graphics card's memory only once, when the mesh is created, and not in every frame.

In our `draw()` method, we used the `PShape` object; we tilted it 30 degrees on the x axis and made it rotate around the y axis by limiting the frame rate to 25 frames per second and adding a small amount to the rotation angle in every frame.

## Let there be light

The second task of our current mission is to turn the mesh we created in the first task into a solid sphere with a smooth surface. We will use different light sources to illuminate our sphere and define so-called normal vectors for each vertex to make our sphere's surface look smooth without the need for a very dense mesh.

## Engage Thrusters

Let's turn on some lights:

- 1. Open the sketch we created for the last task.
- 2. Our mesh is currently hollow, and we can see the front-facing lines as well as the back of the sphere. To change this, we can define a fill color for the faces.

```
PShape makeSphere(int r, int step) {
  PShape s = createShape();

  s.beginShape(QUAD_STRIP);
  s.fill(200);
  s.stroke(255);
  s.strokeWeight(2);
  for (int i = 0; i < 180; i += step) {
    float sini = sin(radians(i));
    float cosi = cos(radians(i));
    float sinip = sin(radians(i + step));
    float cosip = cos(radians(i + step));

    for (int j = 0; j <= 360; j += step) {
      float sinj = sin(radians(j));
      float cosj = cos(radians(j));
      s.vertex(r * cosj * sini, r * -cosi, r * sinj * sini);
      s.vertex(r * cosj * sinip, r * -cosip, r * sinj * sinip);
    }
  }
  s.endShape();
  return s;
}
```

- 3. Run your sketch. The sphere should look like the one in the following screenshot:

![img/5cb67baa7bc7778de360b14c00d1ab81_181_0.png]

- 4. We will now remove the lines on our mesh by calling the `noStroke()` function.

```
PShape makeSphere(int R, int step) {
  PShape s = createShape();

  s.beginShape(QUAD_STRIP);
  s.fill(255);
  s.noStroke();
  for (int i = 0; i < 180; i += step) {
    float sini = sin(radians(i));
    float cosi = cos(radians(i));
```

- 5. Now our sphere has no grid lines anymore; it is rendered completely flat and looks like a white circle (because, by default, only the ambient light is allowed to remain active in Processing). To fix this, we need to add a light source. The first light source we are going to add is a point source. We need to define the color of our light source in the first three parameters, followed by the x, y, and z coordinates of its position. The point light source acts like a very small, but very bright, light bulb. Add the following line to the `draw()` method:

```
void draw() {
  background(0);
  translate(width/2, height/2);
  pointLight(255, 255, 255, -250, -250, 500);
  pushMatrix();
  rotateX(radians(-30));
  rotateY(a);
  a += 0.01;
  shape(sphere);
  popMatrix();
}
```

- 6. Run your sketch. The sphere gets shaded now, but it doesn't look as smooth as we'd like it to be. As you can see in the following screenshot, the quad strips we define run as rings around our sphere; there are visible sharp bends where they touch each other:

![img/5cb67baa7bc7778de360b14c00d1ab81_182_0.png]
