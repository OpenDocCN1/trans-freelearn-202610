Note that we used yet another parameter of the plot function, pch, to choose a different point shape (16 corresponds to the filled circles, while the default argument is a plus symbol +).

There are 26 point shapes available. The possible shapes and their code are listed in the ?points help page.

The following screenshot shows the graphical output that is produced:

![](bbox=[0.25, 0.3, 0.75, 0.66])

We can see that two of the airports fall within (thus, by definition, also intersect) a single county, while the third airports falls within a different county. Using over with airports and nm (in that order) will return the following output:

```
> over(airports, nm)
      NAME_1   NAME_2 TYPE_2  FIPS    area
```