# Create an array

In previous chapters, you created a constant or variable by declaring it and assigning an initial value to it. You can create an array in the same way.

Imagine that your spouse has asked you to get some items from a convenience store. Let’s implement a shopping list using an array. Add the following code to your playground and run it:

```
var shoppingList = ["Eggs", "Milk"]
```

This instruction creates an array variable named shoppingList. The assigned value, ["Eggs", "Milk"], is an array literal. It represents an array with two elements of type String, with "Eggs" at index 0 and "Milk" at index 1.

Using the var keyword here means that the array’s contents can be modified. Since Swift uses type inference, this array’s elements will be of type String.

Imagine that you need to check how many items you need to get at the store. In the next section, you’ll learn how to determine the number of elements in an array.

# Check the number of elements in an array

To find out how many elements there are in an array, use count. Type in and run the following code:

```
shoppingList.count
```

As the shoppingList array contains two elements, 2 is displayed in the Results area.

You can check to see whether an array is empty by using isEmpty. Type in and run the following code:

```
shoppingList.isEmpty
```

As the shoppingList array contains two elements, false is displayed in the Results area.

It is also possible to see whether an array is empty by using shoppingList.count == 0, but using shoppingList.isEmpty offers better performance.

Imagine that your spouse calls and asks you to also get cooking oil, fish, and chicken while you are at the store. In the next section, you’ll see how to add elements to the end of an array, and at a specified array index.