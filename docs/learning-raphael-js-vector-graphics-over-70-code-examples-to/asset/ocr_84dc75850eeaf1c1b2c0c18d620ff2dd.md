```
popDensity: [country.population, country.density],
'stroke-width': 4
});
paper.text(currentX, 225, country.name).attr({
'font-size': 14
});

currentX += 100;
}
```

During each iteration of the loop, the population density attributes are determined
from our data objects and are drawn to our canvas. We have labeled each circle with
the country's name as text and incremented the x-point so that successive circles are
drawn to the right of the previous one. The result is shown here:

![](bbox=[0.233, 0.371, 0.766, 0.569])

As you can see, the most populous countries do not necessarily have the largest
population densities. While Taiwan and the Netherlands have roughly equal
populations, Taiwan has significantly more people per square kilometer than
the Netherlands.

As mentioned, we can animate custom attributes from one set of values to another
set of values. Consider what happens if we set the population and density values
equal to zero for all of our elements when they are initialized in the loop and then
animate to the real values of population and density:

```
var currentX = 70;
for(var i = 0, ii = data.length; i < ii; i+=1) {
var country = data[i];
var circle = paper.circle(currentX, 150, 50).attr({
popDensity: [0, 0],
'stroke-width': 4
});
```