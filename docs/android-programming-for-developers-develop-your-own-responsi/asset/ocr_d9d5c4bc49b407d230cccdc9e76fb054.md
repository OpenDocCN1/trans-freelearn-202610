# Setting up our objects with constructors

With all these private variables and their getters and setters, does it mean that we need a getter and a setter for every private variable? What about a class with lots of variables that need initializing at the start? Think about:

+   - mySoldier.name
+   - mySoldier.type
+   - mySoldier.weapon
+   - mySoldier.regiment
+   - ...

Some of these variables might need getters and setters, but what if we just want to set things up when the object is first created, to make the object function correctly?

Surely, we don't need two methods (a getter and a setter) for each?

Fortunately, this is unnecessary. For solving this potential problem, there is a special method called a **constructor**. We briefly mentioned the existence of a constructor when we discussed instantiating an object from a class. Let's take a look again. Here, we create an object of the type Soldier and assign it to an object called mySoldier:

```
Soldier mySoldier = new Soldier();
```

Nothing new here, but look at the last part of that line of code:

```
...Soldier();
```

This looks suspiciously like a method.

All along, we have been calling a special method known as a constructor that has been created behind the scenes automatically by the compiler.

However, and this is getting to the point now, like a method, we can override it, which means we can do really useful things to set up our new object *before* it is used. This next code shows how we could do this:

```
public Soldier(){
// Someone is creating a new Soldier object
```