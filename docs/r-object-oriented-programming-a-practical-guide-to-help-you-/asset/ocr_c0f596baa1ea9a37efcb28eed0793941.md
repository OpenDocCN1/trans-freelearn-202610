```
Creating a generic function for 'plot' from package 'graphics' in the global environment
Creating a generic function for 'summary' from package 'base' in the global environment
>
> course <- Course()
> course <- SetFileName(course,"shortList.csv")
> course <- ReadGrades(course)
> x <- course['test1']
> summary(x)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  19.00   68.00   76.00   72.56   88.00   98.00
> p <- course['project1']
> summary(p)
A A+ A-  B B+
2  1  1  1  4
>
```

The files available in the repository for this book contain definitions for the show, summary, and plot commands. We examine one more here. The plot command for a NumericGrade object is a bit more complicated than the other examples. The plot command obtains the grades from the object passed to it, and it prints out a histogram, adds a box plot at the top of the histogram, and the rug command is used to display the data values on the same plot.

The arguments to the command include the maximum grade for the assignment. This argument is required because in some circumstances, extra credit is available and some students may achieve a higher score than is allocated for the rest of the class, and in other situations, no student may achieve a perfect score. If this argument is not provided, then the maximum score is used.

A second argument is included that indicates what percentages to use. The default is 10, which indicates that the breaks in the histogram are made at the 10 percent marks for the scores. For example, if the default of 10 is used, then the scores in the 90-100 range are counted together.

The final argument is the ellipses symbol (...), which indicates that other arguments can be passed to the function. The same symbol is used in the plot command within the method. The idea is that all of the extra parameters are passed to the plot command. This allows us to set a wide array of plot parameters through the method without having to catch any special cases:

```
setMethod(f="plot",
          signature="Assignment",
          definition=function(x,maxGrade=NA,div=10,...)
          {

              #print("Plotting an assignment")

              values <- GetValue(x) # Get the raw scores
              if(is.na(maxGrade)) {
                  # The maxGrade was not set. Assume the max score from
                  # the data is the maximum possible.
```