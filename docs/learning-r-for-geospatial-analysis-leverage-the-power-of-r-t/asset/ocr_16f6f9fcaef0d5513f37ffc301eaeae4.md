# Working with Vectors and Time Series

As you can see, the acceptable levels of the resulting factor object (which are, by default, defined as a set of unique values that the vector has) are printed along with its values.

# Using the assignment operator to save
an object

So far we have used R by entering standalone expressions on the command line. As mentioned in the previous chapter, the returned objects are not saved anywhere this way. Therefore, we cannot make sequential operations with each created object serving as an input for the next step(s). However, saving intermediate result is essential to automate processes.

Saving objects to the temporary memory is called assignment. By temporary, we mean that the objects are deleted when we shut down the computer (or quit R), as opposed to writing to a file on the hard drive, where the information will permanently remain unless it's deleted. Assignment is performed by an assignment expression, which is composed of the object we would like to save, the assignment operator =, and the name we would like to give the new object. For example, we can save the 1:10 sequential vector to an object named v as follows:

```
```r
> v = 1:10
```
```

We can then access our newly created object using its name the same way we accessed predefined objects (such as pi) in the previous chapter:

```
```r
> v
[1]  1  2  3  4  5  6  7  8  9 10
```
```