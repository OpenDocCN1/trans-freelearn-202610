The part is now converted to a Sheet Metal part and a number of features appear in the FeatureManager Design Tree (Figure 11.6). We can also see that the lower edges that were previously sharp have now been converted to bends and that Auto Relief cuts have been added to the lower corners.

![](bbox=[0.13, 0.18, 0.78, 0.62])

Figure 11.6: The Insert Bends features and model

Looking at the FeatureManager Design Tree (on the left-hand side of Figure 11.6) shows us a step-by-step guide of the process that has just occurred.

First, we created the cube shape (Boss-Extrude1) and Shelled it out (Shell1). Next, the Sheet-Metal folder was added as part of the Insert Bends process. As usual, this folder always contains all of the Sheet Metal parameters for this part. This is followed by the Rip feature (Rip1) that cut rips down the vertical edges of the cube. The Flatten-Bends operation (Flatten-Bends1) then folded down the sides of the cube, and the Process-Bends operation (Process-Bends1) added the bends into the model and folded the sides back up. Finally, the Flat-Pattern folder was added, and this will allow us to flatten the part at a future stage if needed. If we do flatten the model (Figure 11.7), the relief cuts in the corners will be seen much more easily.