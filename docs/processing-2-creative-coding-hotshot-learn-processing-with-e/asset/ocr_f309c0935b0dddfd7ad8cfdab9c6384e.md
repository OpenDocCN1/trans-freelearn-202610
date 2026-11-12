# # Making Processing see

Our next task is to install the SimpleOpenNI library. This library enables us to use the OpenNI API in Processing. At first, we will learn how to access and display the depth image of the Kinect controller. Then, we will use the user tracking capabilities of the SimpleOpenNI framework and define some callback functions to get notified when a user is detected or when the tracked user disappears. And finally, we will color all the pixels in the depth image that belong to the user to enable the user to see what is being tracked.

# ## Engage Thrusters

Let's teach Processing how to see:

+   1. At the time of writing, the SimpleOpenNI framework could not be installed using the new Library Manager. To install it manually, download the SimpleOpenNI package for your operating system from http://code.google.com/p/simple-openni/ downloads/list.
+   
+   2. Unzip it to a folder named `library` in your `sketchbook` folder. You can find the path to the folder in the **Preferences** dialog.
+   
+   3. Restart Processing if it is currently running, to make sure that the library is found by your installation.
+   
+   4. Now, create a new sketch and click on **Import library ...** under the **Sketch** menu to include the SimpleOpenNI library.
+   
+   5. Add a `setup()` and `draw()` method to your sketch.
+      `import SimpleOpenNI.*;`
+      `void setup() {`
+      `}`
+      `void draw() {`
+      `}`
+   
+   6. In the `setup()` method, resize the sketch window to 640 x 480 pixels and define an OpenNI context object. We use the `enableDepth()` method to start capturing the depth image and `setMirror()` to activate the mirror function.
+      `import SimpleOpenNI.*;`
+      `SimpleOpenNI context;`
+      `void setup() {`
+      `size( 640, 480 );`