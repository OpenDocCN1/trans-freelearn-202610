```
```r
+ }
The value of lupe is  1
The value of lupe is  1.33
The value of lupe is  1.66
The value of lupe is  1.99
```
```

# The repeat loop

A repeat loop is used to denote a block of code that will be repeatedly executed until an explicit breakout of the block is executed. The primary advantage of the repeat loop is that the start of the code block will always be executed. One disadvantage is that it can be difficult to read. The syntax is relatively simple:

```
```r
repeat
    code block
```
```

You must use a break command to tell R to exit the code block. The break command is described in the next subsection in more detail. An example of a repeat loop is given in the following example, and it is a brief example of simulation of a random walk in the complex plane:

```
```r
positions  <- complex(0) # Initialize the history of positions
currentPos <- 0.0+0.0i    # Start at the origin
NUMBERSTEPS <- 50          # Number of steps to take
angleFacing <- 0.0         # direction it is facing
stdDev      <- 1.0         # std dev. of the change in the angle

step <- as.integer(0)
repeat
{
    ## Update the current time step
    step <- step + as.integer(1)

    ## Check to see if it is time to stop
    if(step>MAXNUMBER)
        break

    ## Add new people to the line and update the length
    angle <- angle + rnorm(1,0.0,stdDev)
    currentPos <- currentPos + exp(angle*1.0i)
    positions <- c(positions,currentPos)
}
plot(Re(positions),Im(positions),type="l")
```
```

# Break and next statements

The break and next statements are used to influence which part of the code in the current loop will be executed. The break statement will move to the very end of the current block and it will stop the execution of the loop. The next statement will act as if the end of the code block was reached and start over at the beginning of the code block to begin the next iteration.

As a demonstration, we build on the simulation of the random walk in the previous