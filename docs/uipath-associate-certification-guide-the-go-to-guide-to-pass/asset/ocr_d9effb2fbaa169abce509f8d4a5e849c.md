+   GenericValue variables: GenericValue variables come in handy when you are unsure what type of data is returned from a process. They have the capability of automatically converting their kind according to the first value assigned to the variable. UiPath recommends the use of GenericValue variables as a temporary solution in the workflow until the point you derive the actual data type. GenericValue variables can hold any data type, including String, Number, and arrays. For example, you are comparing two Excel sheets column by column and each column has a different data type. In such scenarios, we can use GenericValue variables to perform a comparison.

+   Collection type variables: Includes all collection objects, such as arrays, lists, and dictionaries. The following screenshot describes commonly used collection objects:

| Collection Type | Description |
|---|---|
| Arrays | Used to store multiple values of the same data type. The size of the object is defined at creation. |
| Lists | Used to store multiple values of the same data type (similar to Arrays). However, the size of the List may change during execution. |
| Dictionary | Used to store data values in the form of key-value pair where its unique key refers to each value. In simple words, every value in a Dictionary variable is referenced using a unique key that describes the value in the collection. |

Figure 5.26 – Collection data types

By now, you should have an initial understanding of commonly used data types. We will discuss how to use these data types and their unique functions in Chapter 7, Manipulating Data Using UiPath.