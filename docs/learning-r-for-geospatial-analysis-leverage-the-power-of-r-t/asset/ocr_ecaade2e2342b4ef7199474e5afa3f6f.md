# Getting help

A help page on every function in R can be reached by using the ? operator (or the help function). For example, the following expression opens the help page for the sqrt function:

```
> ?sqrt
```

The same result is achieved by typing help(sqrt).

![](bbox=[0.22, 0.31, 0.26, 0.35])

> On the other hand, the ?? operator searches the available help pages for a given keyword (corresponding to the help.search function).
> Another useful expression regarding the official R help pages is help.start() that opens a page with links to R's official introductory manuals.

The structure of all help files on functions is similar, usually including a short description of what the function does, the list of its arguments, usage details, a description of the returned object, references, and examples. The help pages can seem intimidating at first, but with time they become clearer and more helpful for reminding oneself of the functions' usage details.

Another important source of information on R is the Internet. Entering a question or a task that we would like to perform (such as Googling r read raster file) into a web search engine usually yields a surprising amount of information from forums, blogs, and articles. Using these resources is inevitable when investigating new ways to utilize R.

# Exploring the basic object types in R

So far, we have encountered two types of objects in R: numeric values (numeric vectors, to be precise, as we will see in Chapter 2, Working with Vectors and Time Series) and functions. In this section, we are going to introduce the key concept that an object is an instance of a certain class. Then, we will distinguish between, for operational purposes, the classes that are used to store data (data structures) and classes that are used to perform operations (functions). Finally, a short sample code that performs a simple GIS operation in R will be presented to demonstrate the way themes introduced in this chapter (and those that will be introduced in Chapter 2, Working with Vectors and Time Series, and Chapter 3, Working with Tables) will be applied for spatial data analysis in the later chapters of this book.