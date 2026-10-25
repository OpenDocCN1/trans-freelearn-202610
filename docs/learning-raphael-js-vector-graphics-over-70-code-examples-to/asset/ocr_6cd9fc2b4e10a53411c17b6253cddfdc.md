![](bbox=[0.277, 0.15, 0.722, 0.325])

You can see that the north-east region—which has the lowest population—is colored white while the south-east region—which has the largest population—is colored black.

One unfortunate effect of London having a population size that is nearly as large as that of the south-east region of the country is that it is impossible to distinguish between the London region and the south-east. This can be addressed in this instance by applying a lighter-colored stroke to each region when looping through our data array:

```
```
path.attr({
    shading: [population, minPopulation, maxPopulation,
    totalPopulations],
    'stroke': '#bbb'
});
```
```

The result is as shown:

![](bbox=[0.291, 0.607, 0.708, 0.808])