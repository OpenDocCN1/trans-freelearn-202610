
You may also notice that the `Tracking FPS` is generally the same as the `Video FPS`. If the `Tracking FPS` is lower, it means there's something wrong with the software; possibly other applications are running, causing Faceware to be under-resourced. Try closing down any other applications that are running to help out with this.

**Note**

The difference between `Tracking FPS` and `Video FPS` is quite simple. `Tracking FPS` is how fast the software can correctly identify areas of the face and where they are in the picture, whereas `Video FPS` is simply how many frames the software is receiving from the webcam per second.

## Face Tracking Model

Referring back to *Figure 9.11*, below `Frame Rate`, the next field is `Face Tracking Model`. The drop-down menu provides two options:

*   `Stationary Camera`: Suitable for webcams where the camera is stationary, but the user's head is moving.
*   `Professional Headcams`: Suitable for professional solutions where the camera is fixed to a head rig and only captures facial expressions rather than head movements. Headcams aren't capable of tracking neck movement, so if the user nods or shakes their head, the data isn't recorded. In this scenario, a motion capture suit is relied upon to capture body motion, including head and neck movement. Typically, both body and facial captures are done simultaneously when professional headcams are being used as it produces a more natural result.

In our case, we are picking `Stationary Camera`.

## Image Rotation

In cases where your camera is upside down or turned to the side, you can rotate the video signal in increments of 90 degrees to fix it. I don't need this, so the `Image Rotation` option is set to `0` degrees.

## Flip Horizontal and Flip Vertical

Again, if you are dealing with an upside-down camera or if you have a personal preference for a mirror image, you can choose to flip the image horizontally or vertically. I won't be using these either.

## Grid Overlay

This is a somewhat helpful tool to ensure your face is in the middle of the frame. I haven't enabled this.

Now, before we go over to the `STREAMING` panel, make sure that you have a video signal coming into Faceware, that your input is set to `Live`, and that your face is being tracked, with tracking markers moving in tandem with your face. You can see from my example in *Figure 9.13* that I have orange tracking markers around my eyebrows, eyes, nose, and mouth:

![Figure 9.13: Tracking markers working](image/B17878_09_13.jpg)

*Figure 9.13: Tracking markers working*

Once your face is being tracked, then you need to ensure that Faceware is able to send data out in the `STREAMING` panel.

## Streaming panel

Switch over to the `STREAMING` panel, and you will see the following options shown in *Figure 9.14*:

![Figure 9.14: The Streaming panel](image/B17878_09_14.jpg)

*Figure 9.14: The Streaming panel*

Let's take a look at each section of the panel.

### Stream to Client

The first option in `STREAM TO CLIENT` is the `STREAM TO CLIENT` toggle button itself, which I have toggled to the right to turn it on. Effectively, the button is just an on-and-off switch. When it's on, Faceware will stream live motion capture data to the client. The client in this instance is the Unreal Engine, which is enabled to receive the streaming data via Live Link.

Note that if you don't have a live video signal, you won't be able to enable `STREAM TO CLIENT`.

The second option is `PORT` – these ports are set up automatically when the Live Link plugin is installed. During the installation of Faceware, Faceware detects which ports are available for streaming to; however, do make note of the port number just in case. You should leave `PORT` as the default value; in my case, that is `802`.

Now you are ready to start the face tracking. Get your face into a neutral pose, so no smiling or pouting, and click on the `CALIBRATE NEUTRAL POSE` button as per the bottom left of *Figure 9.15*:

![Figure 9.15: Calibrating neutral pose](image/B17878_09_15.jpg)

*Figure 9.15: Calibrating neutral pose*

Be sure to look directly into the webcam when you do this and expect to hold that pose for about 5-10 seconds while Faceware recalibrates to your face. When it is done, it will automatically track your face. Take a look at *Figure 9.16*, where you can see that the CGI character is following the movement of my own head and facial expressions.

![Figure 9.16: ANIMATION TUNING panel](image/B17878_09_16.jpg)

*Figure 9.16: ANIMATION TUNING panel*

You'll also notice the `ANIMATION TUNING` panel on the right of the screen; you'll see a lot of horizontal bars moving in response to your head moving. These are all the ARKit controls being animated as a result of the face tracking. We will go into this in further detail later. For now, let's take a further look at the Realtime Setup features.

### Control Schema

`CONTROL SCHEMA` refers to the naming conventions used for the streaming data from Faceware studio. Because we are utilizing ARKit, make sure you enable `ARKIT`. `LEGACY` and `STANDARD` are good, but they don't have the fidelity of ARKit, which is used in the design of MetaHumans, and for this book, they are redundant.

### Stream Face Groups

The `STREAM FACE GROUPS` feature allows us to remove certain parts of the face from streaming. If we were using a professional head cam that doesn't record head rotation, I would suggest disabling the head from `STREAM FACE GROUPS`. In that scenario, we would be capturing the head rotation using a body motion capture solution.

In our example, let's assume that we want to record the head rotation and all the facial movement. To do this, under `STREAM FACE GROUPS`, ensure to enable `ALL`. However, if you just wanted to capture one face group, for example, you just want to move eyebrows, you could just enable the `EYEBROWS` option.

At this point, we should be streaming, but let's just look at one final part of the Faceware interface: the status bar.

## Status bar

In *Figure 9.17*, you can see that the top toolbar is a color-coded tab list providing a way to troubleshoot and rectify issues quickly as they arise:

![Figure 9.17: Top toolbar](image/B17878_09_17.jpg)

*Figure 9.17: Top toolbar*

They are in the order of left to right:

*   `VIDEO`: This is a shortcut to the `REALTIME SETUP` panel, which we have just covered.
*   `TRACKING`: This is a shortcut to the viewports, as seen in *Figure 9.17*, so that we can visualize how well the tracking is going. You can see in *Figure 9.18* that not only is my head movement being tracked, but the tracker is even able to detect a wink through my glasses.

![Figure 9.18: Viewports](image/B17878_09_18.jpg)

*Figure 9.18: Viewports*

*   `TUNING`: This is a shortcut to the `TUNING` panel, where we will do most of the tweaking and fixing of issues. You can see that we have a lot of control over refining the brows, eyes, head, and mouth. As daunting as this may look, these controllers are relatively simple to use.

For example, if you look at the CGI head and its jaw doesn't open as wide as your jaw in the webcam, you can increase the `Jaw Open` parameter to as much as 200% by dragging the blue slider to the right. I made similar adjustments in *Figure 9.19* by dragging some of the mouth sliders to the right as much as 200%.

![Figure 9.19: Tuning](image/B17878_09_19.jpg)

*Figure 9.19: Tuning*

If the jaw opens too much in the CGI character compared to your webcam video, then drag the slider to the left to decrease the amount. The same can be said for all of the parameters in the `TUNING` panel.
