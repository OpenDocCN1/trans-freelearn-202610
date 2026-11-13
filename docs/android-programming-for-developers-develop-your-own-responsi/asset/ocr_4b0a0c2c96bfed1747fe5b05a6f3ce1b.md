calls the constructor of the super class with the value for shieldStrength. We could do further specific Fighter initialization in this constructor, but for now, we just print out the location so we can see when the Fighter constructor is being executed. Because we must, we also implement Fighter specific version of the abstract fireWeapon method. Create a class called Fighter and type this code:

```
public class Fighter extends AlienShip{
    public Fighter(){
        super(400);
        // Strong shields for a fighter
        Log.i("Location: ", "Fighter constructor");
    }

    public void fireWeapon(){
        Log.i("Firing weapon: ", "lasers firing");
    }
}
```

And here is our code in the onCreate method of MainActivity. As usual, enter this code after the call to setContentView. This is the code that uses our three new classes. The code looks quite ordinary and is nothing new; it is the output that is interesting:

```
Fighter aFighter = new Fighter();
Bomber aBomber = new Bomber();

// Can't do this AlienShip is abstract -
// Literally speaking as well as in code
// AlienShip alienShip = new AlienShip(500);

// But our objects of the subclasses can still do
// everything the AlienShip is meant to do

aBomber.shipName = "Newell Bomber";
aFighter.shipName = "Meier Fighter";

// And because of the overridden constructor
// That still calls the super constructor
// They have unique properties
Log.i("aFighter Shield:", ""+ aFighter.getShieldStrength());
Log.i("aBomber Shield:", ""+ aBomber.getShieldStrength());
```