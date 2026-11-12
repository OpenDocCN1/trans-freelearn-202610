# # Path transitions made easy

If we use D3's path generators, it is also quite simple to animate shapes. We can define the starting state as one generator and the ending state as another generator for the transition, and D3 will do the rest for us. Let's look at an example:

```
```
<svg width="800" height="600">
<path class="area" />
</svg>

<script>

var $path = d3.select('.area');
var points = [
	[0, 0], [100, 10], [200, 250],
	[300, 375], [400, 340], [500, 135],
	[600, 360], [700, 280], [800, 480]
];

var area0 = d3.svg.area()
	.y0(600)
	.y1(600)
	.x(function(d) { return d[0]; })
	.tension(1.0)
	.interpolate('cardinal');

var area1 = d3.svg.area()
	.y0(600)
	.y1(function(d) { return 600 - d[1]; })
	.x(function(d) { return d[0]; })
	.interpolate('cardinal')
	.tension(0.6);

$path.datum(points)
	.attr('d', area0)
	.transition()
	.duration(1000)
	.attr('d', area1);

</script>
```
```