# Add a new element to an array

You can add a new element to the end of an array by using append(_:). Type in and run the following code:

```
shoppingList.append("Cooking Oil")
```

"Cooking Oil" has been added to the end of the shoppingList array, which now contains three elements – "Eggs", "Milk", and "Cooking Oil" – and "Eggs", "Milk", and "Cooking Oil" are displayed in the Results area.

You can also add an array to another array with the + operator, using the following code:

```
shoppingList = shoppingList + ["Fish"]
```

You can add a new item at a specified index using insert(_:at:). Type and run the following code:

```
shoppingList.insert("Chicken", at: 1)
```

This inserts "Chicken" at index 1, so now the shoppingList array contains "Eggs", "Chicken", "Milk", "Cooking Oil", and "Fish". Note that "Chicken" is the second element in the array as the first element is at index 0. This can be seen in the Results area:

![](bbox=[0.13, 0.5, 0.865, 0.72])

Figure 5.2: Array contents displayed in the Results area

Imagine that you’ve got the first item on your shopping list, and now you need to know the next item on the list. In the next section, you’ll see how to access a specific array element using the array index.