
you call the UseMethod command, R will search for a function whose class matches the first element in the vector. If it does not find that function, it looks for a function that matches the second element, and it proceeds until it reaches the last element in the vector. If it does not find anything, it calls the default function. With this in mind, we now examine new definitions of the classes. Rather than manually creating the class, we define functions that will create a list representing the class, append a class name to the class attribute, and then return the list. There are three classes, and we will define one function for each class. The first function is used to define a constructor for an object of the GeometricTrial class:

```
GeometricTrial <- function()
{
    # Create the basic data structure - a list that keeps track of
    # a set of trials.

    # Create the basic methods as part of a list to be returned.
    me = list(
        # Define the history to keep track of the trials.
        history = character(0)
    )

    # Define my class identifier and return the list.
    class(me) <- append(class(me),"GeometricTrial")
    return(me)
}
```

Prior to returning the list, the append function is used to add the new class name to the end of the current class attribute. This idea is used in classes that are derived from the GeometricTrial classes as well. The constructor for the Die and Coin classes can now be defined, and both constructors explicitly call the constructor for the parent class, perform any actions associated with the current class, and then append the current class name to the class attribute:

```
Die <- function()
{
    # Define the object by first calling the constructor for the base class
    me <- GeometricTrial()
    # Add the class name to the end of the list of class names
    class(me) <- append(class(me),"Die")
    return(me)
}

Coin <- function()
{
    # Define the object by calling the constructor for the base class
    me <- GeometricTrial()

    # Add the class name to the end of the list of class names
    class(me) <- append(class(me),"Coin")
    return(me)
}
```

The GeometricTrial class includes four methods. The reset method behaves exactly like

the reset method discussed in the previous section. The `getHistory` method is an accessor for a data element and is discussed in the following section. We will now discuss the `simulation` method, and a discussion on the `singleTrial` method will follow.

The `simulation` method is used to simulate a single experiment. The history is first cleared, and the `singleTrial` method is repeatedly called until a successful result is returned. We first define the base simulation function, the default simulation function, and then the simulation function used by the `GeometricTrial` class, as follows:

```
```r
simulation <- function(theObject)
{
    UseMethod("simulation",theObject)
}

simulation.default <- function(theObject)
{
    warning("Default simulation method called on unrecognized object.")
    return(theObject)
}

## Define a method to run a simulation of a geometric trial.
simulation.GeometricTrial = function(theObject)
{
    theObject <- reset(theObject)   # Reset the history
                                     # before the trial.
    repeat
    {
        ## perform a single trial and add it to the history
        thisTrial <- singleTrial(theObject)
        theObject <- appendEvent(theObject,thisTrial$result)
        if(thisTrial$success)
        {
            break   # The trial resulted in a success. Time
                    # to stop!
        } # The trial was not a success. Keep going.

    return(theObject)
}
```
```

The effort to define a default function may not appear to be a worthwhile endeavor. However, this practice is generally employed to ensure that the system can responsibly react if the methods you define are called by mistake.

The final step is to define the `singleTrial` methods. This method is executed by the child classes, Die and Coin. Again, the base and default methods are created. In this case, though, there are also methods for each of the three classes. The base function calls the `UseMethod` function, which scrolls through the `class` attribute for the first function to call. We use a method for the `GeometricTrial` class to demonstrate the order of the calls as well as the `NextMethod` function. The `NextMethod` function continues the search in the `class` attribute and will call the next function based on the class names that follow the current class:

## Encapsulation

The final method for the `getHistory` class will now be defined. It is defined in a separate section to stress an important point. An S3 object is generally a basic data structure, such as a vector or a list that has an additional class attribute defined. The functions that are defined for the class react to the `class` attribute in a predictable way.

One side effect is that every element of an object from a given class is public data. The elements contained within an object can always be accessed. The result is that when programming in R, we must take extra steps to maintain discipline with respect to accessing the data elements maintained by an object. Code that directly accesses data elements within an object may work when first written, but any change to the class constructor risks breaking code in the other methods defined for a class.

With respect to our previous example, we have an accessor, the `getHistory` method. If we have an object, called oneDie, from the Die class, we can easily get the history using `oneDie$history`. If we later decide to change the data structure used to store the history, then any code directly accessing this variable is likely to fail.

Instead, we write an accessor method, `getHistory`, which is designed to return a vector that has the history in the form of a vector of factors. It is important to maintain discipline and only use this method to get a copy of the history. Have a look at the following code:

```
getHistory <- function(theObject)
{
    UseMethod("getHistory",theObject)
}

getHistory.default <- function(theObject)
{
    return(factor()) # Just return an empty vector of factors
}

getHistory.GeometricTrial <- function(theObject)
{
    return(as.factor(theObject$history))
}
```

## A final note

There is one final note to share about S3 classes. If you have used R, you most likely have used them. Many functions are defined to react according to the class name of their first argument. Have a look at the following diagram:

![](img/18eeabe9ab8c81c5a3f4040db5906c67_178_0.png)

A common example of this is the `plot` command. If you type the `plot` command without arguments, you can see its definition, as follows:

```
```r
> plot
function (x, y, ...)
UseMethod("plot")
<bytecode: 0x32fdd50>
<environment: namespace:graphics>
>
```
```

The `plot` command will react differently depending on what kind of object you passed to it. If you wish to see what classes the `plot` command can handle, you can use the `methods` command to list them:

```
```r
> methods(plot)
[1] plot.HoltWinters*   plot.TukeyHSD*       plot.acf*
[4] plot.data.frame*    plot.decomposed.ts*  plot.default
[7] plot.dendrogram*    plot.density*         plot.ecdf
[10] plot.factor*        plot.formula*         plot.function
[13] plot.hclust*        plot.histogram*       plot.isoreg*
[16] plot.lm*            plot.medpolish*       plot.mlm*
[19] plot.ppr*           plot.prcomp*          plot.princomp*
```
```

+   [22] `plot.profile.nls*`  `plot.spec*`  `plot.stepfun`
+   [25] `plot.stl*`    `plot.table*`  `plot.ts`
+   [28] `plot.tskernel*`

**Non-visible functions are asterisked**

> >

One of the greatest advantages of the S3 class definition is that it is simple to build on what is already available. In the example from the previous section, I would like to have the `plot` command react appropriately according to whether or not I pass it a class of the type `Die` or `Coin`. Assuming that I have the previous classes, `Die` and `Coin`, defined, I merely have to define two new plot functions, as follows:

```
```r
> plot.Die <- function(theDie,theTitle)
+     {
+         plot(theDie$getHistory(),
+              xlab="Value After A Die Roll",ylab="Frequency",
+              main=theTitle)
+     }
>
> plot.Coin <- function(theCoin,theTitle)
+     {
+         plot(theCoin$getHistory(),
+              xlab="Value After Coin Flip",ylab="Frequency",
+              main=theTitle)
+     }
> plot(aCoin,"This Here Trial")
> plot(aDie,"A More Better Trial")
```
```

It is common to use this idea to extend a number of commands. Some common examples include the `print` and the `format` functions.
