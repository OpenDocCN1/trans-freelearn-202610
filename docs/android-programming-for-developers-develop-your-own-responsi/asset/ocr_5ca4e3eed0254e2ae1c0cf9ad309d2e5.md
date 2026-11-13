explained shortly.

Hmmm, looks a bit like a method call, right?

Just like regular variables, we could also have done it in two parts, like this:

```
```
Soldier mySoldier;
mySoldier = new Soldier();
```
```

This is how we would assign and use the variables of our hypothetical class:

```
```
mySoldier.health = 100;
mySoldier.soldierType = "sniper";
```
```

```
```
// Notice that we use the object name mySoldier.
// Not the class name Soldier.
// We didn't do this:
// Soldier.health = 100;
// ERROR!
```
```

In the preceding code, the dot operator . is used to access the variables of the class; and this is how we would call the method, again, by using the object name and not the class name and followed by the dot operator:

```
```
mySoldier.shootEnemy();
```
```

We can summarize the use of the dot operator with a diagram: