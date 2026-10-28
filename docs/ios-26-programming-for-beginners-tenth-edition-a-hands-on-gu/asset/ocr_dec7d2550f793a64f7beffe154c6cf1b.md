If you wish to start from scratch, create a new playground and name it CollectionTypes. You can type in and run all the code in this chapter as you go along.

To find out more about arrays, dictionaries, and sets, visit https://docs.swift.org/swift-book/documentation/the-swift-programming-language/collectiontypes.

The first collection type you will learn about is arrays, which let you store information in an ordered list.

# Arrays

Let’s say you want to store the following:

+   - List of items to buy at a convenience store
+   - Chores that you must do every month

Arrays would be suitable for this. An array stores values in an ordered list. Here’s what it looks like:

| Index | Value |
| --- | --- |
| 0 | value1 |
| 1 | value2 |
| 2 | value3 |

Figure 5.1: Representation of an array

Values must be of the same type. You can access any value in an array by using the array index, which starts at 0.

If you create an array using the let keyword, its contents can’t be changed after it has been created. If you want to change an array’s contents after creation, use the var keyword.

Let’s see how to work with arrays. You’ll create an array by assigning a value to it in the next section.