# # Iterating over our timers and starting the animation

To initialize the animation, we iterate over our timers, defining our `counts` text with an initial value of `0` positioned at the center-x and center-y point of our arc. We then run our `update` function on each timer, which has the effect of animating the arcs in a recursive manner as we have discussed.

```
```
for(var i = 0, ii = timers.length; i < ii; i+=1) {
	(function(i) {
		// Draw descriptive text beneath the timer
		paper.text(timers[i].cx, timers[i].cy + timers[i].r + 30,
		timers[i].text);
		// Define counts text
		counts[i] = paper.text(
			timers[i].cx, timers[i].cy, '0'
		).attr({
			'counts': [0, timers[i].total]
		});
		update(i);
	})(i);
}
```
```

# # Supplementary material

Two additional data visualization demos have been written up at `http://raphaeljsvectorgraphics.com` and are available to you as supplementary material.

# # Facebook usage by year

A simple timeline visualization has been created to demonstrate the rise in the number of users by year. The timeline is a Catmull-Rom curve that can be panned in the x-direction as shown in the following screenshot: