# # Assignment

The R environment is an interactive shell. Commands are entered using the keyboard, and the environment should feel familiar to anyone used to MATLAB or the Python interactive interpreter. To assign a value to a variable, you can usually use the = symbol in the same way as these other interpreters. The difference with R, however, is that there are other ways to assign a variable, and their behavior depends on the context.

Another way to assign a value to a variable is to use the <- symbols (sometimes called operators). At first glance, it seems odd to have different ways to assign a value, but we will see that variables can be saved in different environments. The same name may be used in different environments, and the name can be ambiguous. We will adopt the use of the <- operator in this text because it is the most common operator, and it is also the least likely to cause confusion in different contexts.

The R environment manages memory and variable names dynamically. To create a new variable, simply assign a value to it, as follows:

```
```r
> a <- 6
> a
[1] 6
```
```

A variable has a scope, and the meaning of a variable name can vary depending on the context. For example, if you refer to a variable within a function (think subroutine) or after attaching a dataset, then there may be multiple variables in the workspace with the same name. The R environment maintains a search path to determine which variable to use, and we will discuss these details as they arise.

The <- operator for the assignment will work in any context while the = operator only works for complete expressions. Another option is to use the <<- operator. The advantage of the <<- operator is that it instructs the R environment to search parent environments to see whether the variable already exists. In some contexts, within a function for example, the <- operator will create a new variable; however, the <<- operator will make use of an existing variable outside of the function if it is found.

Another way to assign variables is to use the -> and ->> operators. These operators are similar to those given previously. The only difference is that they reverse the direction of assignment, as follows:

```
```r
> 14.5 -> a
> 1/12.0 ->> b
> a
[1] 14.5
> b
[1] 0.08333333
```
```