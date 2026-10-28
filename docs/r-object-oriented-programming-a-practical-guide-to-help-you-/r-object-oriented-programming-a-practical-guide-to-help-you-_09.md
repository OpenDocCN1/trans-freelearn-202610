## 第 11 章 案例研究——模拟

我们现在将检查一组设计用于实现蒙特卡洛模拟的 S3 类。示例脚本包含一个用于生成单次模拟的类，以及一个用于运行并收集多次模拟结果的类。模拟类包含两个派生类。一个用于离散随机过程的模拟，另一个用于随机微分方程的模拟。

本节大致分为以下部分：

- 模拟类
- 蒙特卡洛类
- 示例

我们首先检查一组设计用于生成随机系统单次模拟的模拟类。这些类使用一个基类来管理参数。从基类派生出两个类。第一个派生类用于模拟离散随机系统。第二个派生类用于近似随机微分方程。

在检查模拟类之后，使用一个主类来管理蒙特卡洛模拟。蒙特卡洛类接受一个模拟类，并收集多次模拟的结果。我们的重点是这些类的基本结构，因此我们不讨论数据的统计方法。

我们将在本章最后一节简要检查一个使用这些类的示例。该示例重点介绍如何从离散随机模拟类创建对象，以及如何生成结果。




### 模拟类

模拟类由三部分组成。Simulation 类是基类，DiscreteSimulation 和 ContinuousSimulation 两个类派生自基类。我们假设这些类的定义保存在单独的文件 simulationS3.R 中。

基类 Simulation 用于管理单个模拟的参数和结果。数据包括模拟中使用的最终时间，并为数据定义了访问方法：

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

请注意，该类包含一个附加方法 getFinalValues，用于检索模拟中近似的最后值。该方法由下一节中定义的 Monte-Carlo 类所必需。结果用于确定所需的统计量。

接下来的两个类是 `DiscreteSimulation` 和 `ContinuousSimulation` 类。`DiscreteSimulation` 类用于生成以下方程给出的离散随机系统的近似值：

$x_{n+1}=ax_n+bx_ny_n+N_1W_1,\ y_{n+1}=gx_n+dy_n+N_2W_2$

在前述方程中，a、b、N1、g、d 和 N2 为常数，W1 和 W2 为均值为零、标准差为一的正态分布随机变量。`ContinuousSimulation` 类用于使用 Milstein 格式生成随机微分方程的近似值：

$dx=(axy+by)dt+N_1dW_1,\ dy=(gx+dy)dt+N_2dW_2$

在前述方程中，a、b、N1、g、d 和 N2 为常数，W1 和 W2 为独立的维纳过程。

所有类的完整脚本都可以在随本文本提供的代码中找到。出于简洁考虑，我们只查看一个派生类：`DiscreteSimulation` 类。该类的定义如下所示：

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

该类特有的最后一个方法是 `singleSimulation` 方法，用于进行一次模拟并保存结果。定义离散模拟类所需的方法如下：

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

### 蒙特卡洛类

最后检查的类是 MonteCarlo 类。MonteCarlo 类用于跟踪多次模拟的结果。在此，我们提供该类的部分代码列表，然后提供用于生成多次模拟的方法。

首先，给出定义该类所需的代码：

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

该类还需要一个额外的方法。simulations 方法用于创建多次模拟并记录结果：

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

### 总结

定义了一个 S3 类的示例，可用于跟踪一组模拟。类中包括一个用于创建单个模拟的单独类。单个模拟可以是对离散或连续随机过程的近似。还开发了另一个类，可以跟踪大量模拟的结果。

在下一章中，我们将研究另一个扩展示例。下一章的重点是创建一组 S3 类，以提供一种通用方法来处理各种数据类型回归任务。您可以从 https://www.packtpub.com/sites/default/files/downloads/66820S_Case_Study_Regression 下载本章

