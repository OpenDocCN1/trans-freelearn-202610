# Sets

Let’s say you’re writing a Movies app, and you want to store a list of movie genres. You could do this with a set.

A set stores values in an unordered list. Here’s what it looks like:

| Value |
| --- |
| value1 |
| value2 |
| value3 |

Figure 5.4: Representation of a set

All values are of the same type.

If you create a set using the let keyword, its contents can’t be changed after it has been created. If you want to change the contents after creation, use the var keyword.

Let’s look at how to work with sets. You’ll create a set by assigning a value to it in the next section.

# Create a set

Imagine that you are creating a Movies app, and you would like to store movie genres in your app. The app will store movie genres that you like, and it can check to see whether a movie that you’re thinking of seeing is among them. Compared to arrays, sets are unordered and only contain unique values, whereas arrays are ordered by index and can contain duplicates. Since you do not need to store movie genres in order and each genre is unique, you will use a set for this purpose.

As you have seen for arrays and dictionaries, you can create a set by declaring it and assigning a new value to it. Add the following code to your playground and run it:

```
var movieGenres: Set = ["Horror", "Action", "Romantic Comedy"]
```

This instruction creates a set variable named movieGenres. Note that the set literal assigned to it, ["Horror", "Action", "Romantic Comedy"], has the same format as an array literal, so you use type annotation to set the type of movieGenres to Set. Otherwise, Swift’s type inference will create an array variable and not a set variable.

Using the var keyword here means that the set’s contents can be modified. This set’s elements will be of type String due to type inference.