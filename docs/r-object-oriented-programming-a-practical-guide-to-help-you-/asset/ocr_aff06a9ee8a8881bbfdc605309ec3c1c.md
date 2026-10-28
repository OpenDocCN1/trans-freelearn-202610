The final topic to examine in this section is the use of the `[` operation to set the value of an entry. The approach is similar to the methods used to get information given earlier. The only difference is that instead of using the setMethod function, we use the setReplace method, and the last argument to the function is the value to set the corresponding entry in the appropriate object:

```
setReplaceMethod("[",
                 signature("NumericGrade"),
                 definition=function(x,i,value)
                 {
                     #print(paste("grade value",i,value))
                     x@value[i] = value
                     return(x)
                 }
                 )

setReplaceMethod("[",
                 signature("Course"),
                 definition=function(x,i,j,value)
                 {
                     #print(paste("course grade",i,j,value))
                     grades <- x@Grades[[i]]
                     grades[j] <- value
                     x@Grades[i] <- grades
                     return(x)
                 }
                 )
```

With these definitions in place, a value within the course for a specific grade can be easily set, as follows:

```
> source('grades.R')
> source('ops.R')
> source('overriding.R')
>
> course <- Course()
> course <- SetFileName(course,"shortList.csv")
> course <- ReadGrades(course)
> print(course['test1'])
[1] Assignment: Test
[1] (9) Grades:
[1] 75 68 98 76 96 81 19 52 88
> print(course['test1',3])
[1] 98
>
> course['test1',3] <- 99.1
> print(course['test1',3])
[1] 99.1
>
```