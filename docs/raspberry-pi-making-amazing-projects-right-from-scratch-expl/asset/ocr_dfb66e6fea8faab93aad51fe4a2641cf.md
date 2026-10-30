# Taking a snap

You can now test the webcam by capturing a still image, which can be done by running the following command:

```
$ fswebcam test.jpg
```

You should expect to see output similar to the following:

![](bbox=[0.177, 0.267, 0.822, 0.476])

> fswebcam has lots of options for things like the resolution and quality of the image. Use the command fswebcam -? to get a list of all options.

# Snap snap snap

fswebcam doesn't take video streams, but you can set it up to take a series of frames at regular intervals. For example, to take a snap every 10 seconds, you can use the following command:

```
$ fswebcam frame.jpg -l 10
```

An example of how this would be useful can be demonstrated by setting the webcam to take a snap every few seconds in the background (the -q switch runs fswebcam in the background). When our security system is triggered, we could then take the latest image snapped with the webcam which could be looking down your pathway.

For the purpose of putting together our entire system later in this book, we'll be focusing on the Raspberry Pi Camera Module, but you can always replace the code with the previous examples if you want to use USB webcams instead.