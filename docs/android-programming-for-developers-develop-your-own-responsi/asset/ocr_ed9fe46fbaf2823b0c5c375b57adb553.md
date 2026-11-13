```
// we can use the public setter setHealth()
mySoldier.setHealth(100);//That's better

Hospital militaryHospital = new Hospital();

// Oh no mySoldier has been wounded
mySoldier.setHealth(10);
```

```
/*
 Take him to the hospital
 But my health variable is private
 And Hospital won't be able to access it
 I'm doomed - tell Laura I love her

 No wait-what about my public getters and setters?
 We can use the public getters and setters from another class
 */
```

```
militaryHospital.healSoldier(mySoldier);
```

```
// mySoldiers private variable health has been increased by 10
// I'm feeling much better thanks!
```

We see that we can call our public setHealth and getHealth methods directly on our object of the type Soldier. Not only that, we can call the healSoldier method of the Hospital object, passing in a reference to the Soldier object, which can also use the public getters and setters to manipulate the private health variable.

We see that the private health variable is simply accessible, yet totally within the control of the designer of the Soldier class.

If you want to play around with this example, there is code for a working app in the code bundle in the chapter 9 folder called GettersAndSetters. I have added a few lines of code to print to the console. We deliberately covered this the way we did to keep the key parts of the code as clear as possible.

# Note

Getters and setters are sometimes referred to by their more correct names, **accessors** and **mutators**. We will stick to getters and setters. I just thought you might like to know the jargon.