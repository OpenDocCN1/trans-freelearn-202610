

- **Variable name** This is the name of the data element used in your program.
- **Business source element** This is the mapping of the variable to a business element. If the variable is a new or transformed variable, list the logic that is used to create or transform the variable.
- **Type of variable** This indicates the type of variable: nominal, interval, binary, continuous, and so on.
- **Variable role** This indicates whether it is a potentially dependent, independent, or informational variable.

The total sales include Widget1 up until 2016.

**Additional metadata** 10% missing values in sales for Northeast due to system glitch in February 2016.

As you begin to explore your dataset, you can also add additional columns, such as the percentage of missing values, variable means, extreme values, and so on.

## SQL

SQL has been referred to as a universal query language. One of the reasons to learn SQL is that it has a relatively common syntax that runs on a variety of operating systems. Most importantly, knowledge of SQL is paramount for communicating ideas with managers who are familiar with it. While queries can also be done natively in R via native tools and packages, I prefer SQL as a first query language since work performed using SQL in R can then be modified to run natively within other systems, where the code can be optimized in its native environment, in order to make it run faster.

## Example: Using SQL to get sales by region

In this example, we will use the R package `sqldf` to show the difference in sales and units in four sales regions. Note that this is an artificial example since we will first generate separate data for the `West` region with sales figures that are higher than the other three regions:

```
install.packages("sqldf")
library(sqldf)
set.seed(10)
rows=100
y <- rbind(
data.frame(indv=factor(paste("TransId-", 1:100, sep = "")),
Sales=rnorm(rows, mean=150000, sd=100000),
Units=round(rnorm(rows,mean=10, sd=3)),
Region=sample(c("North","East","South"),rows, replace=TRUE),
data.frame(indv=factor(paste("TransId-", 101:200, sep = "")),
Sales=rnorm(rows, mean=2000000, sd=1000000),
Units=round(rnorm(rows,mean=10, sd=3)),
Region=sample(c("West"),rows, replace=TRUE))
)
query <- "select Region,avg(Sales),avg(Units) from y group by Region"
results <- sqldf(query,stringsAsFactors = FALSE)
results
```

The results will appear in the console window:

```
> results
Region avg(Sales) avg(Units)
1 East 1493504 9.333333
2 North 1487338 10.457143
3 South 1477877 9.250000
4 West 2016626 10.270000
```

## Note

This is randomly generated data. The functions `rnorm()` and `sample()` in the written code are giveaways. Generating random data is a great way to begin to test code and algorithms since you will always have a better idea of what kind of results to expect, given the assumptions.

## Charts and plots

At the business understanding phase, it is not necessary to get too complex with visualizations. The reason I say this is that in some instances complex visualization can influence interpretation, rather than provide objective data. Basic charts and plots, which indicate the relationship between the variables are more than adequate for communicating ideas.

When passing charts and plots back and forth during this phase, it is a good idea to include basic statistical measures of distribution and association. This will give management a rough idea of how significant the relationships are, rather than having them rely purely on the visual representation.

## Spreadsheets

Sending the data to spreadsheets and creating pivot tables is another way to get management involved in the process. Spreadsheets have definite disadvantages, but they are heavily used in the industry and knowing how to manipulate spreadsheets will be another way that you can communicate with managers.

## Simulation

**Simulation** is a tool which can demonstrate what-ifs. This technique is very useful if there is not a lot of historical data to go by, and you need to make assumptions about the behavior of some of your variables. Typically, you do not have a lot of data at this point, so you are really constructing your own data, based upon how you think it will behave.

In the later sections of the book, I will use simulation techniques heavily to illustrate this.

## Example of simulating if a customer contact will yield a sale

Here is an example that is based upon a business estimate that one of three customer contacts will result in a sale. Another assumption is that if a sale is made, it will result in an average of $100 each with a standard deviation of $5.

ExpectedPayoff either produces 0 revenue or a figure that hovers around $100, as specified in line 5 of the following code:

```
library(ggplot2)
set.seed(123)
CustomerAcquired.Flag <- sample(c(0,0,1), 100, replace = TRUE)
Revenue <- sample(rnorm(100,100,5))
ExpectedPayoff <- CustomerAcquired.Flag*Revenue
head(ExpectedPayoff)
PayoffCompare = ggplot(data.frame(ExpectedPayoff), aes (x=Expected PayoffCompare
```

> head(ExpectedPayoff)
[1] 0.00000 101.26659 0.00000 100.58823 97.12327 0.00000

We can view this conditional customer acquisition problem as two separate charts contained within one visualization. The left side of the visualization is the count of customer contacts which result in no revenue, and the right side shows the histogram of binned sales when they occur:

![](img/672c0cf8ba3c000a6529b117ed96945e_70_0.png)

## Example: simulating customer service calls

In this example, we can simulate calls to the customer service which might occur at the beginning and at the end of the week. Management projects that weekend volume will be heavy, with an average of 1500000 calls and an average turnaround time of 4 minutes. Customer service call volume for Monday-Thursday is estimated as handling 1000000 calls with an average turnaround time of 1 minute. We could use this simulation to build a model to include weekends versus nonweekends as new variables in a predictive model:

```
library(ggplot2)
library(grid)
library(gridExtra)
set.seed(123)
MonTuesWedThurs=rnorm(1000000,1,1)
FriSatSun=rnorm(1500000,4,1)
weekly = c(MonTuesWedThurs,FriSatSun)
```

If we were interested in looking at the difference in call volumes, we could look at them individually (as shown in the top row of the following plot) or as a combined distribution for weekends and nonweekends. The combined plot is one way of illustrating the difference between weekends and nonweekends, and shows that they have similar shapes, but different mean values:

```
p1 = ggplot(data.frame(FriSatSun), aes(x=FriSatSun)) + stat_bin(b
p2 = ggplot(data.frame(MonTuesWedThurs), aes(x=MonTuesWedThurs))
p3 = ggplot(data.frame(weekly), aes(x=weekly)) + stat_bin(binw
grid.arrange(p1, p2, p3, ncol = 2)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_71_0.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_71_1.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_71_2.png)

## Step 2 data understanding

Once an objective is established and data sources have been identified, you can begin looking at the data in order to understand how each data element behaves individually, as well as how it interacts in combination with other variables. But even before you start looking at the values of variables, it is important to understand the different types of data levels of measurement and the kind of analyses you can perform with them.

## Levels of measurement

Levels of measurement is a classification system for classifying data into 4 different categories which is discussed as follows (ratio, ordinal, interval, and nominal). It is an important aspect of the project or studies metadata.

Levels of measurement is important in the world of predictive analytics since the specific measurements will often dictate which algorithm or techniques can be applied. For example k-means clustering does work if you want to incorporate nominal data, and logistic regression can not use ratio data as a dependent variable (although you can transform variables to a lower scale).

## Nominal data

Nominal data, sometimes referred to as categorical data, simply defines a class or group without any natural order. Nominal variables are usually character data, but not always. Your gender, the cell phone brand you prefer, and the type of book you are reading now are all examples of nominal variables. Sometimes it can be tricky, since the number 1 and 2 written on a sign are also nominal variables when they are used to designate two possible waiting lines you may need to visit at the motor vehicle bureau. Nominal data only designates classes and you cannot perform any mathematical operations on them, such as addition or subtraction.
