Notice the white pixels in our threshold image showing where the ball is located. You can add more OpenCV code that gives the actual location of the ball. In our original image file of the ball's location, you can actually draw a rectangle around the ball as an indicator. Edit the file to look as follows:

![](bbox=[0.23, 0.22, 0.77, 0.56])

The added lines look like the following:

+   - `hue_image = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)`: This line creates a hue image out of the RGB image that was captured. Hue is easier to deal with when trying to capture real world images; for details, refer to http://www.bogotobogo.com/python/OpenCV_Python/python_opencv3_Changing_ColorSpaces_RGB_HSV_HLS.php.
+   - `threshold_img = cv2.inRange(hue_image, low_range, high_range)`: This creates a new image that contains only those pixels that occur between the low_range and high_range n-tuples.