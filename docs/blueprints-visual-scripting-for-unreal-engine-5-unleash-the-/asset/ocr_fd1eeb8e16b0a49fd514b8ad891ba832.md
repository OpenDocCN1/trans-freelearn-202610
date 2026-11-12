# Figure 5.38 – Adding the Set Timer by Event node

+   8. Right-click to open the context menu and add Event BeginPlay.
+   Drag the TimeToChange variable from the My Blueprint panel and choose the Get TimeToChange option. Connect the nodes as shown
+   in the following screenshot:

![](bbox=[0.125, 0.235, 0.865, 0.618])

# Figure 5.39 – The timer will run the ChangeDirection Event periodically

+   9. Compile and save the Blueprint.

# Testing moving targets

Now that we have updated our Blueprint, we can test to ensure that the
BP_CylinderTarget instance moves as expected. First, we must place the
BP_CylinderTarget instance in a position that allows it to move along the y
axis without bumping into other objects. The coordinates I used were 220
on the x axis, -600 on the y axis, and 220 on the z axis.