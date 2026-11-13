# # Arrays are objects

We said that arrays are reference variables. Think of an array variable as an address to a group of variables of a given type. Perhaps, using the warehouse analogy, someArray is an aisle number. So, someArray[0], someArray[1], and so on are the aisle numbers followed by their position numbers in the aisle.

Arrays are also objects, that is, they have methods and properties that we can use, for example:

```
int lengthOfSomeArray = someArray.length;
```

Here, we assigned the length of someArray to the int variable called lengthOfSomeArray.

We can even declare an array of arrays. This is an array that, in each of its elements, lurks another array. The declaration is shown as follows:

```
String[][] countriesAndCities;
```

In the preceding array, we could hold a list of cities within each country. Let's not go array crazy just yet though. Just remember that an array holds up to a predetermined number of variables of any predetermined type, and these values are accessed using this syntax:

```
someArray[someLocation];
```

Let's actually use some arrays to try and get an understanding of how to use them in real code and what we might use them for.