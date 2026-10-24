You might have noticed that attributes() shows us the attribute values, but we don't know the field names yet. To get the field names, we use this code:

```
for field in v_layer.fields():
    print field.name()
```

The output is as follows:

ID
fk_region
ELEV
NAME
USE

Once we know the field names, we can access specific feature attributes, for example, NAME:

```
for feature in v_layer.getFeatures():
    print feature.attribute('NAME')
```

This is the output:

NOATAK
AMBLER
BETTLES
...

A quick solution to, for example, sum up the elevation values is as follows:

```
sum([feature.attribute('ELEV') for feature in v_layer.getFeatures()])
```

Here is the output:

22758.0

> In the previous example, we took advantage of the fact that Python allows us to create a list by writing a for loop inside square brackets. This is called list comprehension, and you can read more about it at https://docs.python.org/2/tutorial/data structures.html#list-comprehensions.

![](bbox=[0.22, 0.67, 0.275, 0.71])