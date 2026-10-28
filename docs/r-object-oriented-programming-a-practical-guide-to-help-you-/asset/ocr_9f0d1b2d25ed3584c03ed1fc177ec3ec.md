# Polymorphism

The definition of S4 classes allows methods to be overloaded. That is, multiple functions that have the same name can be defined, and the function that is executed is determined by the arguments’ types. We will now examine this idea in the context of defining the methods used to set the activity level in the Ant class.

Two or more functions can have the same name, but the types of the arguments passed to them differ. There are two methods to set the activity level. One takes a floating point number and sets the activity level based to the value passed to it. The other takes a logical value and sets the activity level to zero if the argument is FALSE; otherwise, it sets it to a default value.

The idea is to use the signature option in the setMethod command. It is set to a vector of class names, and the order of the class names is used to determine which function should be called for a given set of arguments. An important thing to note, though, is that the prototype defined in the setGeneric command defines the names of the arguments, and the argument names in both methods must be exactly the same and in the same order:

```
setGeneric(name="SetActivityLevel",
           def=function(antie,activity)
           {
               standardGeneric("SetActivityLevel")
           }
)

setMethod(f="SetActivityLevel",
          signature=c("Ant","logical"),
          definition=function(antie,activity)
          {
              if(activity) {
                  antie@ActivityLevel = 0.1
              } else {
                  antie@ActivityLevel = 0.0
              }
              return(antie)
          }
)

setMethod(f="SetActivityLevel",
          signature=c("Ant","numeric"),
          definition=function(antie,activity)
          {
              if(activity>=0.0) {
                  antie@ActivityLevel = activity
              } else {
                  warning("The activity level cannot be negative")
              }
              return(antie)
          }
)
```

Once the two methods are defined, R will use the class names of the arguments to