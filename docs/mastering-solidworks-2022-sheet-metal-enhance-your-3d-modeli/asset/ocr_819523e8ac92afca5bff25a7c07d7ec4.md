Therefore, be aware that Jog offset distances may sometimes be different from what you think you have specified:

![](bbox=[0.15, 0.15, 0.85, 0.42])

Figure 3.29: Sometimes, the Jog offset distance will be greater than what was specified

11. To reduce the offset distance, we must either reduce the Bend Radius value or adjust the Jog Angle value.

Edit the Jog feature and reduce the Bend Angle from 90° using the down arrow next to the Offset Distance box. Notice that as the angle reduces, the Offset Distance value in the preview also reduces, because we are effectively reducing the height of the bends in the Jog.

At some point (in this case, 60°), the gap in the preview will no longer reduce. This shows us the maximum angle that we can use to achieve the correct offset distance of 1.5mm.