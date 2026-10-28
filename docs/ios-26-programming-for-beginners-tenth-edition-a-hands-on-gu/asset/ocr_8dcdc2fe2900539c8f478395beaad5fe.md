Since the initializer for the Animal class does not have a parameter to assign a value to hasFurOrHair, the default value is used, and true will be displayed in the Debug area.

You have seen that a subclass can have additional properties. A subclass can also have additional methods, and method implementation in a subclass can differ from the superclass implementation. Let’s see how to do that in the next section.

# Override a superclass method

So far, you’ve been using multiple print() statements to display the values of the class instance. You’ll implement a description() method to display all the instance properties in the Debug area, so multiple print() statements will no longer be required. Follow these steps:

+   1. Modify your Animal class declaration to implement a description() method, as shown:

```
class Animal {
    var name: String
    var sound: String
    var numberOfLegs: Int
    var breathesOxygen: Bool = true
    init(name: String, sound: String, numberOfLegs:
    Int, breathesOxygen: Bool) {
        self.name = name
        self.sound = sound
        self.numberOfLegs = numberOfLegs
        self.breathesOxygen = breathesOxygen
    }
    func makeSound() {
        print(sound)
    }
    func description() -> String {
    "name: \(name) sound: \(sound)
    numberOfLegs: \(numberOfLegs)
    breathesOxygen: \(breathesOxygen)"
    }
}
```