# # ImageView

We have already put an image onto our layout a couple of times so far, but we have never got a reference to image from our Java code or done anything with it before. The process is exactly the same. Follow the next steps to get a reference to ImageView as you would to any other widget:

+   1. Declare an object
+   2. Get a reference using the findViewById method and a valid id property like this:

```
ImageView imageView = (ImageView) findViewById(R.id.imageView);
```

Then, we can go on to do some neat things with our image using code like this:

```
// Make the image 50% TRANSPARENT
imageView.setAlpha(.5f);
```

# Note

The odd-looking f term simply lets the compiler know that the widget's value is of the type float, as required by the setAlpha method.

In the preceding code, we used the setAlpha method on imageView. The setAlpha method takes a value between 0 and 1. Completely see-through is 0 and no transparency at all is 1.

# Tip

There is also an overloaded setAlpha method that takes an int value from 0 (completely see-through to 255, with no transparency). We can choose whichever value is the most appropriate at the time. If you want a reminder about method overloading, refer to Chapter 8, Coding in Java Part 2 – Methods.

We will use some of the ImageView methods in our next mini app.