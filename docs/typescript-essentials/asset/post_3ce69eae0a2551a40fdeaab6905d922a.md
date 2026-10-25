
![](img/83a7f7e1723158e56008cf70304a8da4_120_0.png)

As you can see, we are now able to provide all of our parameters in an easily managed list. The following screenshot shows how to use this file with the compiler:

![](img/83a7f7e1723158e56008cf70304a8da4_120_1.png)

## Summary

In this chapter, we covered each of the different parameters for the TypeScript compiler. We discussed how to target the different versions of ECMAScript as well as which external module pattern to follow. Source maps will become very handy to debug our TypeScript code and declaration files will help us deploy compiled JavaScript while still providing a rich type experience for developers using TypeScript. The ability to pass a file to the compiler allows us to create reusable compilation scripts that can scale much further than using the command line alone. Next, we are going to cover how the different constructs turn what is normally a scripting language into an object-oriented language.

## The basics

Before we dig into the main concepts of OOP, let’s talk about what objects are and how they work. Objects are self-contained entities that maintain a set of data members and methods. The data members are unique to each object instance and the methods have direct access to these members. An object represents an instance of a class type and any number of instances can be created. Each of these instances will have different memory locations to store their internal member variables at runtime to differentiate between them. The following code sample shows a class definition and then the creation of different objects of that type:

```
class Shape {
    public locationX: number = 0;
    public locationY: number = 0;
    public height: number = 0;
    public width: number = 0;
    constructor() {
    }
    public draw() {
    }
    public resize(height: number, width: number) {
        this.height = height;
        this.width = width;
    }
    public move(x: number, y: number) {
        this.locationX = x;
        this.locationY = y;
    }
}

var shapeInstance = new Shape();
var otherShape = new Shape();
shapeInstance.move(30, 35);
otherShape.resize(15, 15);
```

As you can see, we have a simple Shape class that contains a few data members as well as a few methods. We are then able to instantiate multiple instances of this class and store references to them so that we can operate on them separately. As you can see, we change the location of our first object and the size of the second object. If we were to access these members directly, shapeInstance would still have a height and width of 0 and location of (30, 35), while otherShape will still be located at (0, 0) with a height and width of 15.

## SOLID – object-oriented design

SOLID is an acronym for five of the basic principles when designing classes in OOP. Following these principles will ensure that your application is well structured and flexible enough for the future. Let’s take a look at what each of these letters represents:

- **Single Responsibility Principle:** A class should have only one reason to change. If a class has multiple responsibilities, then those responsibilities become coupled together. When this coupling occurs, it increases the potential for a change to one of the responsibilities to break the functionality of the other responsibilities. For more information on this principle, refer to: http://www.objectmentor.com/resources/articles/srp.pdf.
- **Open/Closed Principle:** An object should be open for extension but closed for modification. This principle is the basis for abstraction, which we will discuss briefly. Essentially, your programs should be designed around a specific set of contracts or abstract classes that should be extended to implement new functionality as requirements change. For more information on this principle, please refer to: http://www.objectmentor.com/resources/articles/ocp.pdf.
- **Liskov Substitution Principle:** An object should be able to be replaced with a subtype object without altering program execution. This principle builds on the Open/Closed Principle in that any subclass must maintain the minimum contract established by the base class. However, this principle goes a step further in that it applies to the client use of each object and the behavior expected from the base class. For more information on this principle, please refer to: http://www.objectmentor.com/resources/articles/lsp.pdf.
- **Interface Segregation Principle:** Many client specific interfaces are better than a single general purpose interface. This principle is important because it helps to decouple different behaviors from each other, allowing clients to only implement exactly what they need to. For more information on this principle, please refer to: http://www.objectmentor.com/resources/articles/isp.pdf.
- **Dependency Inversion Principle:** Rely on abstractions rather than concretions. This principle states that proper abstractions should be created that both dependent functions can rely on and concrete implementations will adhere to. When both are capable of using this abstraction, then it becomes simple to reuse code as requirements change with minimal impact. For more information on this principle, please refer to: http://www.objectmentor.com/resources/articles/dip.pdf.

Keeping these principles in mind while developing your applications will allow you to develop object-oriented code with the procedural language of JavaScript.

### Understanding inheritance

Inheritance is the ability of a class to extend the functionality of an existing class. Inheritance creates a way for multiple objects to share a common core set of code and then extend or modify this as necessary for a specific purpose. Take for example our Shape class, which is very simple with a couple of properties in it, but these properties don’t provide specific details as to what shape is actually being represented. In the following code, you can see we have modified the Shape class to be more generic and created a subclass using the extends construct in TypeScript:

```typescript
interface IShape {
    location: IPoint;
    move(newLocation: IPoint);
}
class Shape implements IShape {
    public location: IPoint = new Point(0, 0);
    constructor() {
    }
    public move(newLocation: IPoint) {
        this.location = newLocation;
    }
}
interface IRectangle extends IShape {
    height: number;
    width: number;
    area(): number;
    resize(height: number, width: number);
}
class Rectangle extends Shape implements IRectangle {
    public height: number = 0;
    public width: number = 0;
    constructor() {
        super();
    }
    public area(): number {
        return this.height * this.width;
    }
}
```

As you can see, we have a generic interface for the Shape class that implements only the basic functionality needed for a shape object, which is a location on a plane. The Point class is the same one we created back in Chapter 2, TypeScript Basics. Then a new interface gives us the properties and methods needed to define a basic rectangle shape. This interface is not explicitly necessary, but having it defined will allow us to extend or replace the Rectangle class easily in code later if we need to. This is followed by the class definition for a rectangle which, like the IRectangle definition, shows the usage of the extends keyword. This tells the compiler that the Rectangle class is a shape and should inherit all of the properties that come with it. Inside the constructor, an extra line must be added that initializes the base class instance.

#### Tip

The call to the base class constructor must be the first statement inside the subclass constructor otherwise a compile-time error will occur.

Now every Rectangle object created will have not only the height, width, and area members but it will also contain a Location property and the Move method that are defined by the Shape class. This is not a default construct in the language of JavaScript so let’s take a quick look at the output provided by the compiler to see how this is implemented:

```javascript
var __extends = this.__extends || function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() { this.constructor = d; }
    __.prototype = b.prototype;
    d.prototype = new __();
};
var Point = (function () {
    function Point(x, y) {
        if (typeof x === "undefined") { x = 0; }
        if (typeof y === "undefined") { y = 0; }
        this.x = x;
        this.y = y;
    }
    return Point;
})();

var Shape = (function () {
    function Shape() {
        this.location = new Point(0, 0);
    }
    Shape.prototype.move = function (newLocation) {
        this.location = newLocation;
    };
    return Shape;
})();

var Rectangle = (function (_super) {
    __extends(Rectangle, _super);
    function Rectangle() {
        _super.call(this);
        this.height = 0;
        this.width = 0;
    }
    Rectangle.prototype.area = function () {
        return this.height * this.width;
    };
    return Rectangle;
})(Shape);
```

As you can see, all of our classes are created as modules as discussed previously, but before any of these are generated, the compiler injects a special segment of code to handle extending classes. The TypeScript compiler implements object-oriented principles for us by applying a standard set of well-known patterns—member addition and prototype copying. This code takes in two object types as parameters. The first parameter is the subclass that is in need of all of the base class’s properties, and the second is the base class itself. This function then loops through each of the available members on the base class
