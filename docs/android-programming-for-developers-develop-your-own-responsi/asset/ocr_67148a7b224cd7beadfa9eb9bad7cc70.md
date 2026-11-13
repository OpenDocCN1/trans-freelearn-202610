canvas to draw our graphics on.

We can make a virtual canvas using the Canvas class from any Android UI element. In our demo app, we will draw onto ImageView, and when we make our game, we will draw straight onto a special type of view, which will bring some extra advantages, as we will see.

So, to get started, we need a widget to draw on. Assuming there is ImageView in the layout, we could do the following to get a reference to it:

```
ImageView ourView = (ImageView) findViewById(R.id.imageView);
```

Now, we need a bitmap. A bitmap itself has a coordinate system much like the screen. We are creating a bitmap to turn it into our drawing canvas:

```
Bitmap ourBitmap = Bitmap.createBitmap(300, 600,
Bitmap.Config.ARGB_8888);
```

The previous line of code declares and creates an object of the type Bitmap. It will have a size of 300 x 600 pixels. We will keep this in mind when we draw on it shortly.

# Tip

The last argument in the createBitmap method, Bitmap.Config.ARGB_8888, is simply a format. If you want to know more about bitmap formats, you can do so here: https://en.wikipedia.org/wiki/BMP_file_format.

Now, we can prepare Bitmap for drawing by creating a Canvas object with it:

```
Canvas ourCanvas = new Canvas(ourBitmap);
```

Next, we get ourselves an object of the type Paint. Literally, we can think of this object as both the brush and paint for our virtual canvas:

```
Paint paint = new Paint();
```

At this point, we are ready to use our Paint and Canvas objects to do some drawing. The actual code to draw a pixel in the top-left corner of the screen will look like this:

```
ourCanvas.drawPoint(0, 0, paint); // How simple is that?
```