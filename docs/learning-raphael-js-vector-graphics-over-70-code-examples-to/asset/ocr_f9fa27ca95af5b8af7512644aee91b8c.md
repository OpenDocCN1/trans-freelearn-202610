In this instance, the attribute value is `path`, which is quickly animated to its end point and then back again until finally coming to rest. The change in the path with regard to time is illustrated here:

![](bbox=[0.1777, 0.2066, 0.8223, 0.3779])

# # Custom easing using the cubic Bézier format

The format for CSS3 transitions describing animation easing can be used to define custom easing paths in Raphaël. The syntax is `cubic-bezier(point1_x, point1_y, point2_x, point2_y)`, where each parameter defines the _x_ and _y_ positions of anchor points describing a cubic Bézier curve from (0, 0) to (1,1) in attribute value and time. Consider the easing path that describes an attribute value that increases very sharply and then almost comes to a stop before finally easing in to its end point:

![](bbox=[0.1777, 0.556, 0.8223, 0.7812])