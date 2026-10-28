```
```r
# Define the worker ant class.
WorkerAnt <- setClass(
    # Set the name of the class
    "WorkerAnt",

    # Name the data types (slots) that the class will track
    slots = c(
        Foraging = "logical",    # Whether or not the ant is actively
                                  # looking for food

        Alarm = "logical"        # Whether or not the ant is actively
                                  # announcing an alarm.
    ),

    # Set the default values for the slots. (optional)
    prototype=list(
        Foraging = FALSE,
        Alarm    = FALSE
    ),

    # Make a function that can test to see if the data is consistent.
    # (optional)
    # This is not called if you have an initialize function defined!
    validity=function(object)
    {
        print("Validity: WorkerAnt")
        return(TRUE)
    },

    # This class inherits from the FemaleAnt class
    contains="FemaleAnt"
)
```
```

When a new worker is created, it inherits from the FemaleAnt class:

```
```r
> worker <- WorkerAnt(Position=c(-1,3,5),Length=2.5)
> worker
An object of class "WorkerAnt"
Slot "Foraging":
[1] FALSE

Slot "Alarm":
[1] FALSE

Slot "Food":
[1] 0

Slot "Length":
[1] 2.5

Slot "Position":
[1] -1  3  5

Slot "pA":
[1] 0.05
```
```