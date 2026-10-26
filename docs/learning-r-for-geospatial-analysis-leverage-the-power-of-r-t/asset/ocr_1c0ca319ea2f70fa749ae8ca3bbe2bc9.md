# Everything is an object

R is an object-oriented language; accordingly, everything in R is an object. Objects belong to classes, with each class characterized by certain properties. The class to which an object belongs to determines the object's properties and the actions we can do with that object. To use an analogy, a gray Mitsubishi Super-Lancer model 1996 object belongs to the class car. It has specific attributes (such as color, model, and manufacturer) for each of the data fields a car object has. It satisfies all criteria that the car class entails; thus, the actions that are applicable to cars (such as igniting the engine and accelerating or using the breaks) are also meaningful with that particular object. In much the same way, a multi-band raster object in R will obligatorily have certain properties (such as the number of rows and columns, and resolution) and applicable actions (such as creating a subset of only the first band or calculating an overlay based on all bands).

All objects that are stored in memory can be accessed using their names, which begin with a character (without quotes; some functions, such as all arithmetic and logical operators can be called using their respective symbol within quotes, such as in "*" as we saw earlier). For example, sqrt is the name of the square root function object; the class to which this object belongs is function. When starting R, a predefined set of objects is loaded into memory, for example, the sqrt function and logical constant values TRUE and FALSE. Another example of a preloaded object is the number π:

```
```r
> pi
[1] 3.141593
```
```

The class function returns the class name of the object that it receives as an argument:

```
```r
> class(TRUE)
[1] "logical"
> class(1)
[1] "numeric"
> class(pi)
[1] "numeric"
> class("a")
[1] "character"
> class(sqrt)
[1] "function"
```
```