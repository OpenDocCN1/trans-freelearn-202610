In the preceding example, we used the `cardinal` interpolation, which is a similar interpolation technique used in Bézier curves. If we look at the resulting line in the following figure, we see that the lines between the data points are now interpolated:

![](bbox=[0.102, 0.122, 0.93, 0.442])

Output of an interpolated line

The smoothness of the interpolation can be controlled by the `.tension(value)` function. In the following figure, we can see the difference between tension 1, which is the edgy inner line, to tension 0.5, which is the smooth outer line:

![](bbox=[0.283, 0.562, 0.744, 0.832])

Line with different tension values