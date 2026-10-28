
## The simulation classes

The simulation classes consist of three parts. The Simulation class is the base class, and two classes, DiscreteSimulation and ContinuousSimulation, are derived from the base class. We assume that the definitions for these classes are kept in a separate file, simulationS3.R.

The base class, Simulation, is used to manage the parameters and results for a single simulation. The data includes the final time used in the simulation and accessor methods are defined for the data:

```
########################Create the base simulation class
##
## This is used to represent a single simulation
Simulation <- function()
{

    ## Create the list used to represent an
    ## object for this class
    me = list(
        simulationResults = matrix(0)
        )

    ## Set the name for the class "Simulation"
    class(me) <- append(class(me),"Simulation")
    return(me)

}# Set the data values that are the result of a simulation.
setSimulation <- function(theSimulation)
{
    UseMethod("setSimulation",theSimulation)
}
setSimulation <- function(theSimulation,simulationResults)
{
    ## Set the value of the variable theSimulation
    theSimulation$simulationResults <- simulationResults
    return(theSimulation)
}
```
## method to return the data from the current set of results.
getFinalValues <- function(theSimulation)
{
    UseMethod("getFinalValues",theSimulation)
}
getFinalValues <- function(theSimulation)
{
    ## Get the value of the data pair at the last time step
    size <- dim(theSimulation$simulationResults)
    return(c(theSimulation$simulationResults[size[1],1],
             theSimulation$simulationResults[size[1],2]))
}
```

Note that the class includes an addition method, getFinalValues, which is used to

retrieve the last values approximated in the simulation. This method is required by the Monte-Carlo class defined in the next section. The results are used to determine the requisite statistics.

The next two classes are the `DiscreteSimulation` and `ContinuousSimulation` classes. The `DiscreteSimulation` class is used to generate an approximation to the discrete stochastic system given by the following equation:

$x_{n+1}=ax_n+bx_ny_n+N_1W_1,\ y_{n+1}=gx_n+dy_n+N_2W_2$

In the preceding equation, a, b, N1, g, d, and N2 are constants, and W1 and W2 are normally distributed random variables with a mean of zero and a standard deviation of one. The `ContinuousSimulation` class is used to generate an approximation using the Milstein scheme to the stochastic differential equation:

$dx=(axy+by)dt+N_1dW_1,\ dy=(gx+dy)dt+N_2dW_2$

In the preceding equation, a, b, N1, g, d, and N2 are constants, and W1 and W2 are independent Wiener processes.

The complete scripts for all of the classes can be found in the code that accompanies this text. In the interest of brevity, we only look at one derived class: the `DiscreteSimulation` class. The definition of the class is given here:

```
###############################################
## Create a simulation for a discrete simulation.
##
## This is used to represent the results from a discrete simulation.
DiscreteSimulation <- function()
{
    ## Define the base class and get the environment
    me <- Simulation()
    me$N <- 0

    ## Set the name for the class with a numeric grade associated with
    ## it.
    class(me) <- append(class(me),"DiscreteSimulation")
    return(me)
}
```

The final method specific to this class is the `singleSimulation` method that is used to conduct one simulation and save the results. The methods necessary to define the discrete simulation class are as follows:

```
# the methods to do the actual simulations.
singleSimulation <- function(simulation,N,T,x0,y0,alpha,beta,
    gamma,delta, noiseOne,noiseTwo)
{
```

```
UseMethod("singleSimulation",simulation)
}

singleSimulation.DiscreteSimulation <-function(
  simulation,N,T,x0,y0,alpha,beta,gamma,delta,noiseOne,noiseTwo)
{
  ## Make an approximation for one run of the discrete model
  ## with the given parameters. Store the approximation in
  ## the simulation slot when done.

  ## initialize the necessary variables.
  x <- matrix(data=double(N*2),nrow=N,ncol=2)
  x[1,1] <- x0
  x[1,2] <- y0
  lupe <- 2

  ## Go through and make N iterations of the stochastic model.
  while(lupe <= N)
    {
      dW <- rnorm(2,mean=0,sd=1)      # Generate two random numbers
                                      # with a normal dist.
      ## Take one step of the discrete model
      x[lupe,1] <- alpha*x[lupe-1,1] + beta*x[lupe-1,1]*x[lupe-1,2] +
        noiseOne*dW[1]
      x[lupe,2] <- gamma*x[lupe-1,1] + delta*x[lupe-1,2] +
        noiseTwo*dW[2]
      lupe <- lupe + 1
    }

  ## Save the simulation and return the result.
  simulation <- setSimulation(simulation,x)
  return(simulation)
}
```

## The Monte-Carlo class

The final class examined is the MonteCarlo class. The MonteCarlo class is used to keep track of the results from multiple simulations. Here, we provide a partial list of the code for the class and then provide the methods used to generate multiple simulations.

First, the code required to define the class is given here:

```
###############################################
# Create the Monte Carlo class
#
# This class is used to make many simulations
MonteCarlo <- function()
{

    # Define the slots
    me = list(

        ## First define the parameters for the stochastic model
        N         = 0,
        T         = 0,
        x0        = 0,
        y0        = 0,
        alpha     = 0,
        beta      = 0,
        gamma     = 0,
        delta     = 0,
        noiseOne  = 0,
        noiseTwo  = 0,

        ## Define the data to track and the number of trials
        xData = 0,
        yData = 0

    )
    ## Set the name for the class
    class(me) <- append(class(me),"MonteCarlo")
    return(me)
}

# Define the method used to initialize the data prior to a run.
prepare <- function(monteCarlo,number)
{
    UseMethod("prepare",monteCarlo)
}
prepare.MonteCarlo <- function(monteCarlo,number)
{
    ## Set the number of trials and initialize the values to
    ## zeroes.
    monteCarlo$xData <- double(number)
    monteCarlo$yData <- double(number)
    return(monteCarlo)
}
```

The class requires an additional method. The simulations method is used to create multiple simulations and record the results:

```
```r
simulations <- function(monteCarlo,number,simulation)
{
    UseMethod("simulations",monteCarlo)
}

simulations.monteCarlo <- function(monteCarlo,number,simulation)
{
    ## Set the number of trials and initialize the values
    monteCarlo <- prepare(monteCarlo,number)
    params <- getParams(monteCarlo)  # get the parameters

    ## Perform the simulations
    lupe <- 0
    while(lupe < number)
    {
        lupe <- lupe + 1 # increment the count
        ## Perform a single simulation.
        simulation <- singleSimulation(
            simulation,
            params[1],params[2],params[3],params[4],params[5],
            params[6],params[7],params[8],params[9],params[10])
        ## Get the last values of the simulation and record them.
        values <- getFinalValues(simulation)
        monteCarlo <- setValue(monteCarlo,values[1],values[2],lupe)
    }

    return(monteCarlo)
}
```
```

## Summary

An example of an S3 class was defined that can be used to keep track of a set of simulations. The classes include a separate class used to create a single simulation. The single simulation can be an approximation of either a discrete or continuous stochastic process. Another class is developed that can keep track of the results from a large number of simulations.

In the next chapter, we look at another extended example. The focus in the next chapter is on creating a set of S3 classes to provide a general way to handle regression tasks for a variety of data types. You can download this chapter from https://www.packtpub.com/sites/default/files/downloads/66820S_Case_Study_Regression
