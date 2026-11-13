# The class implementation

Here is real code for our hypothetical class. We call it a class implementation. Because the class is called Soldier, if we implement this for real, we would do so in a file called Soldier.java:

```
public class Soldier {
    // Member variables
    int health;
    String soldierType;

    // Method of the class
    void shootEnemy(){
    // bang bang
    }
}
```

The code snippet in the preceding example is the implementation for a class called Soldier. There are two **member variables** or **fields**, an int variable called health and a String variable called soldierType.

There is also a method called shootEnemy. The method has no parameters and a void return type, but class methods can be of any shape or size as we discussed in [Chapter 8, Coding in Java Part 2 – Methods].

To be precise about member variables and fields, when the class is instantiated into a real object, the fields become variables of the object itself, and we call them **instance** or **member** variables.

They are just variables of the class, whichever fancy name they are referred to by. However, the difference between fields and variables declared in methods (called **local** variables) does become more important as we progress. We briefly discussed variable scope at the end of [Chapter 8, Coding in Java Part 2 – Methods]. We will look at all types of variables again later in this chapter.