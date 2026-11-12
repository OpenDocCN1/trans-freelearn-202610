
- 7. To fix this, we need to define normal vectors for each vertex, pointing away from the sphere's surface, as shown in this figure:

![](img/5cb67baa7bc7778de360b14c00d1ab81_183_0.png)

- 8. We have already calculated the sine and cosine values we need for the normal vectors. We will now add the normal vectors to our mesh using the normal() method. Each normal vector has to be defined before the vertex() method in our makeSphere() method.

```
PShape makeSphere( int r, int step) {
PShape s = createShape();

s.beginShape(QUAD_STRIP);
s.fill(255);
s.noStroke();
for( int i = 0; i < 180; i+=step ) {
float sini = sin( radians( i ));
float cosi = cos( radians( i ));
float sinip = sin( radians( i + step ) );
float cosip = cos( radians( i + step ) );

for( int j = 0; j <= 360; j+=step ) {
float sinj = sin( radians( j ));
float cosj = cos( radians( j ));

s.normal( cosj * sini, -cosi, sinj * sini);
s.vertex( r * cosj * sini, r * -cosi, r * sinj * sini);

s.normal( cosj * sinip, -cosip, sinj * sinip);
s.vertex( r * cosj * sinip, r * -cosip, r * sinj * sinip);
}
}

s.endShape();
return s;
}
```

- 9. Run your sketch. The sphere should look smooth, like in the following screenshot:

![](img/5cb67baa7bc7778de360b14c00d1ab81_184_0.png)

- 10. Currently, half of our sphere has a dark shadow, so we need a method to make the lighting more even. Fortunately, Processing has a shortcut method named `lights()`, which sets up a light source pointing from the camera to the origin. So, we will now change our `draw()` method and replace the point light source with a call to the `lights()` method.

```
void draw() {
  background(0);
  translate( width/2, height/2 );
  lights();
  pushMatrix();
  rotateX( radians( -30 ) );
  rotateY( a );
  a+= 0.01;
  shape( sphere );

  popMatrix();
}
```

- 11. Run your sketch. The sphere should look like this:

![](img/5cb67baa7bc7778de360b14c00d1ab81_184_1.png)

## Objective Complete - Mini Debriefing

In the second task of our current mission, we turned the wireframe mesh of our sphere into a solid smooth sphere and activated some lights. In steps 1 to 5, we turned off the line strokes and activated the filling of the faces. We also activated a point light source to prevent the sphere from being rendered completely flat.

Our sphere was shaded and filled, but we had sharp bends at each of the quad strip rings that we created using our loops in the `makeSphere()` method. To fix this, we added normal vectors to each vertex. These vectors stand at right angles to the surface and are needed to shade our sphere's surface smooth.

Finally, we learned how to light our sphere more evenly so that every part of it can be seen; this is because in the third task, we will turn our sphere into a globe, and we want every part of the world to be visible.

## From sphere to globe

The third task of our current mission is to convert our sphere into a globe by using a texture image. We will need to define the mapping of each vertex to a part of the texture image to make it fit. We will use a black and white texture, where all the land mass is black and the sea is white. The image was created based on a vector map licensed under a Creative Commons license by STUDIO7DESIGNS, which can be found at `http://vector4free.com/vectors/id/75`. I added Antarctica and converted it to a PNG file.

After this, we will add satellite images provided by NASA's Visible Earth project (`http://visibleearth.nasa.gov/`). We will also add a callback handler for keyboard events and make the textures switchable.

## Engage Thrusters

Let's start texturing our globe:

- 1. First we need to add an image of our globe to the sketch. Download the image from the download section at www.packtpub.com/support. The map looks like this:

![](img/5cb67baa7bc7778de360b14c00d1ab81_186_0.png)

- 2. Drag the image to the sketch window or add it using the Add File... option under the Sketch menu.

- 3. Now we need to define a PImage variable to hold the texture image and load it in our setup() method.

```
PShape sphere;
PImage world;
void setup() {
 size(400, 400, P3D);
 frameRate(25);
 world = loadImage( "world.png" );
 sphere = makeSphere( 150, 5, world);
}
```

- 4. We extend our makeSphere() method to take the texture as a parameter and adjust the call to makeSphere() in our setup() method.

```
void setup() {
 size(400, 400, P3D);
 frameRate(25);
 world = loadImage( "world.png" );
 sphere = makeSphere( 150, 5, world);
}
```

```
PShape makeSphere( int r, int step, PImage tex ) {
```

- 5. Now let's activate the texture in our mesh.

```
PShape makeSphere( int r, int step, PImage tex ) {
PShape s = createShape();

s.beginShape(QUAD_STRIP);
s.texture( tex );
s.noStroke();
```

- 6. If we run the sketch now, we would not be able to see anything because only a small part of the texture would be used (which is black in color). To fix this, we need to add u and v coordinates to each vertex to define which part of the texture should be used. Since we have defined our sphere mesh using two loops (which we use to iterate over the angles of our polar coordinates), we can simply map them to our u and v coordinates.

```
PShape makeSphere( int r, int step, PImage tex ) {
PShape s = createShape();

s.beginShape(QUAD_STRIP);

s.texture( tex );
s.noStroke();
for( int i = 0; i < 180; i+=step ) {
    float sini = sin( radians( i ) );
    float cosi = cos( radians( i ) );
    float sinip = sin( radians( i + step ) );
    float cosip = cos( radians( i + step ) );

for( int j = 0; j <= 360; j+=step ) {
    float sinj = sin( radians( j ) );
    float cosj = cos( radians( j ) );
    float sinjp = sin( radians( j + step ) );
    float cosjp = cos( radians( j + step ) );

s.normal( cosj * sini, -cosi, sinj * sini);
s.vertex( r * cosj * sini, r * -cosi, r * sinj * sini,
tex.width-j * tex.width / 360, i * tex.height / 180);

s.normal( cosj * sinip, -cosip, sinj * sinip);
s.vertex( r * cosj * sinip, r * -cosip, r * sinj * sinip,
tex.width-j * tex.width / 360, (i + step ) * tex.height /
180);
            }
        }
        s.endShape( );
        return s;
    }
```

- 7. Run your sketch. The sphere should look like the one in this screenshot:

![](img/5cb67baa7bc7778de360b14c00d1ab81_188_0.png)

- 8. Now we are going to add two more textures and make them switchable.
First, download the image files bluemarble.jpg and night.jpg from
www.packtpub.com/support. These images were created using images
from NASA's Landsat 2, Terra, and Aqua satellites, and are available for download
at http://visibleearth.nasa.gov/. I have reduced the size, as the original
ones were too detailed for our small globe. This is what the blue marble texture
looks like:

![](img/5cb67baa7bc7778de360b14c00d1ab81_188_1.png)

And this is the earth-by-night texture:

![](img/5cb67baa7bc7778de360b14c00d1ab81_189_0.png)

- 9. We add the images to our sketch by dropping them on the sketch window or by going to Sketch | Add File...

- 10. Now we will add two more PImage variables in our sketch and load the images in the setup() method.

```
PShape sphere;
PImage world;
PImage bluemarble;
PImage night;

void setup() {
size(400,400,P3D);
frameRate(25);
world = loadImage( "world.png" );
bluemarble = loadImage( "bluemarble.jpg" );
night = loadImage( "night.jpg" );
sphere = makeSphere( 150, 5, world);
}
```

- 11. Now we need to replace the texture of our sphere object in the keyPressed() method. We can do this by using the setTexture() method of the PShape class, which replaces the default texture. Since we only use one texture image at a time for our sphere, this works fine for us.

```
void keyPressed() {
if ( key == 'l' ) {
sphere.setTexture( world );
} else if ( key == '2' ) {
  sphere.setTexture( bluemarble ) ;
} else if ( key == '3' ) {
  sphere.setTexture( night ) ;
}
}
```

- 12. Now, run our sketch and switch the textures using the number keys. When using the blue marble texture, the globe looks like this:

![](img/5cb67baa7bc7778de360b14c00d1ab81_190_0.png)

- 13. When the earth-by-night texture is active, the globe looks like this:

![](img/5cb67baa7bc7778de360b14c00d1ab81_190_1.png)
