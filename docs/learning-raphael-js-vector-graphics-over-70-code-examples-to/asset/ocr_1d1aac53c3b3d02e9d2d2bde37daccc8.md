# # Transformations and Event Handling

Here we have combined the commands. In this instance, the transformations are mutually independent meaning that the case of each command is unimportant. However, consider rotation about a point:

```
```
triangleCopy.transform("t 100 0 r 180 220 100");
```
```

This transformation first translates our triangle by 100px in _x_ and then rotates it by 180 degrees clockwise. Our rotation point (220, 100) is not the point that the triangle is rotated about because the rotation point is effectively translated 100px in _x_ to the new point (320, 100). The important point to note here is that _the lowercase transformation command takes the previous transformation into account_:

![](bbox=[0.207, 0.3195, 0.791, 0.4773])

In the case of the absolute rotation command, R, rotation about the point (220, 100) likewise occurs after translation but the results are markedly different as shown in the following diagram given here. The `t 100,0 R 180,220,100` transformation string always rotates an element about the point (220, 100) irrespective of the previous transformations:

![](bbox=[0.207, 0.5898, 0.791, 0.748])