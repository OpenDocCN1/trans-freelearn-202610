object is hit. It just prints to the console and then detects if that particular object's shieldStrength is zero. If it is, it calls the destroyShip method that we'll look at next:

```
```java
public void hitDetected(){
	shieldStrength -=25;
	Log.i("Incoming: ","Bam!!");
	if (shieldStrength == 0){
		destroyShip();
	}
}
```
```

And lastly, for our AlienShip class, we have the destroyShip method. We print a message that indicates which ship has been destroyed based on its shipName, as well as decrement the numShips static variable so we can keep track of how many objects of the type AlienShip we have:

```
```java
private void destroyShip(){
	numShips--;
	Log.i("Explosion: ", ""+this.shipName + " destroyed");
} // End of the class
```
```

Now, we switch over to our MainActivity class and write some code that uses our new AlienShip class. All the code goes in the onCreate method after the call to setContentView. First, we create two new AlienShip objects called girlShip and boyShip:

```
```java
// every time we do this the constructor runs
AlienShip girlShip = new AlienShip();
AlienShip boyShip = new AlienShip();
```
```

Look how we get the value in numShips. We use the getNumShips method as we might expect. However, look closely at the syntax. We are using the class name and not an object. We can also access static variables with methods that are not static. We did it this way to see a static method in action:

```
```java
// Look no objects but using the static method
Log.i("numShips: ", "" + AlienShip.getNumShips());
```
```

Now, we assign names to our public shipName String variables:

```
```java
// This works because shibName is public
```
```