# # Mission Accomplished

Our current mission was to use the neon globe we created in the previous project and turn it into a geographic information system showing where the requests for a website are coming from. We started by parsing the logfile using regular expressions and extracting the IP address and the timestamp from each line. To make the timestamps easier to work with, we used `SimpleDateFormatter` to convert the `string` representation to a `long`. In the `draw()` method, we created a timeline showing the timestamps of a logfile.

In the second task, we added the database from the hostIP project, which allows us to map blocks of IP addresses to geo-coordinates on city level. To speed up the geocoding, we created `HashMap` using the IP address block as a key. Since the loading of the hostIP database takes some time, we moved the initialization code to a separate thread and added a progress bar to our `draw()` method.

In the third task, we used the geocoded data to draw a small red dot for each request on a map. The map we used is the texture image we used for our neon globe in the last project. To make our map look fancier, we replaced the red dots with a vector graphic of a pin.

Our final task for this mission was to use the globe we created in the previous project and to add our visualization to it. We moved the code of initialization of the globe to our new sketch. Then we adapted our `draw()` method to make sure the two-dimensional elements like the progress bar get drawn on top of the globe. Instead of the red dots, we used yellow lines that stick out of the globe's surface. These lines are also drawn as neon lines if our GLSL filters are active.

# # You Ready to go Gung HO? A Hotshot Challenge

In this mission, we created geographic visualizations on a 2D map and a 3D globe, but geographic information systems are a very wide and interesting field, and we certainly have only scratched the surface. Why don't you try to reach the next level and try one of the following ideas:

+   - Allow the user to rotate the globe using the mouse
+   - Use different colored pins for the requested file types
+   - Parse your logfile in real time using a server process and send live updates to your globe
+   - Visualize where your spam mails are coming from
+   - Use the zoomable vector maps for your visualizations