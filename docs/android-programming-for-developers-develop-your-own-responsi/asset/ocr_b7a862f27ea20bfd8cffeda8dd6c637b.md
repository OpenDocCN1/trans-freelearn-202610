value for health as well as soldierType.

Here is the output:

```
Rambo's health =: 150
Vassily's health =: 50
Wellington's health =: 100
Green Beret: is shooting
Sniper: is shooting
Sailor: is shooting
```

Notice that each time we access the health variable of each soldier object, it prints the value we assigned it, demonstrating that although the three objects are of the same type, they are completely separate, individual instances/objects.

Perhaps more interesting is the three calls to shootEnemy. One by one, each of our Soldier object's shootEnemy methods is called and we print the soldierType variable to the console. The method has the appropriate value for each individual object, further demonstrating that we have three distinct objects (instances of the class), albeit created from the same Soldier class.

We saw how each object is completely independent of the other objects. However, if we imagine whole armies of soldier objects in our app, then we realize we are going to need to learn new ways of handling large numbers of objects (and regular variables, too).

Think about managing just 100 separate soldier objects. What about when we have thousands of objects? In addition, this isn't very dynamic. The way we are writing the code at the moment relies on us (the developers) knowing the exact details of the soldiers that the generals (the user) will be commanding. We will see the solution for this in Chapter 13, Handling and Displaying Arrays of Data.