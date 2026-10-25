
We include the references to jQuery and Knockout that will sit on the global namespace and then we have a script tag for require. Included in the tag for require is a binding called `data-main` that is used to define the entry point for the application.

**Note**

For more information regarding RequireJS please visit [http://requirejs.org/](http://requirejs.org/).

In the body of the HTML, we have labels and inputs defining our UI and binding to each of the different objects in our view model. The search results section is doing something new that is worth noting. Using the `foreach` binding on the `searchResult` observable array, we have created a template for every item in the array. The context for these bindings is based on the type of the item being iterated over, in our case a Person object.

While the `firstName`, `lastName`, and `age` properties are not observables that can be bound to, the binding will only occur once. If the value of these fields change, the UI will not be updated. The last thing we need to do is create the entry point of our application. We will need to import the view model we created, wait for the DOM to be ready, and apply our bindings:

```javascript
import PersonFinderViewModel = require("Scripts/PersonFinderViewModel");
$(document).ready(() => {
    ko.applyBindings(new PersonFinderViewModel.PersonFinderViewModel());
});
```

Now we can run the application and add several users to the directory. When we perform a search for someone by their first name, all of the matching results will be displayed:

![](img/83a7f7e1723158e56008cf70304a8da4_197_0.png)

## Chapter 7. Enhancing the Drawing Application

When we started the drawing application in Chapter 5, *Creating a Simple Drawing Application*, we grouped all of our code together by functionality. This allowed us to reduce the number of script tags required for our application to work. Using what we learned in the previous chapter, we will modify the drawing application to use AMD modules and RequireJS. We will also integrate Knockout and jQuery to improve the overall user experience. Finally, we will look at how we can use RequireJS to build all of our application code into a single minified file for deployment optimization. The enhancements we will make to our application include:

- Converting to AMD modules
- Binding user controls
- Generating a single output file
- Styling the application

### Converting to AMD modules

Converting to AMD modules is a fairly simple process. However, this is a good time to look at the structure and maintainability of our project. Before getting started, we must install the RequireJS NuGet package Version 2.1.14 and the corresponding declaration file requirejs.TypeScript.DefinitelyTyped Version 0.2.0.

![](img/83a7f7e1723158e56008cf70304a8da4_202_0.png)

The TypeScript build settings must also be changed to use the AMD module system as shown in the previous chapter. Once these steps are completed, we can begin enhancing our application. To do this, we will want to add a folder structure to our application that will help us easily separate modules based on functionality. At this point, it is important to decide how you want to divide up the code for your application. Since we are using RequireJS modules, we will be using the export keyword for all objects that we want to use from other modules. There are three ways to use the export keyword, and each one has different ramifications on the resulting JavaScript. The following example shows the more common use of the keyword:

```
export class Freehand implements IFreehand {
    public points: Array<IPoint> = [];
    constructor() {
    }
    public AddPoint(point: IPoint) {
        this.points.push(point);
    }
}
```

In this example, the export keyword annotates the type declaration. This tells the TypeScript compiler to place this type on the exports object of the module being generated.

**Tip**

The export keyword is only available when the `--module` flag is provided to the compiler. An attempt to use it without providing this option will result in a compiler error. Visual Studio provides this flag when you select a module system in the TypeScript Build section of the project properties.

The resulting JavaScript from this class definition will look very similar to our previous module definitions. However, there will be some special syntax added since we have chosen to use AMD modules:

```
define(["require", "exports"], function(require, exports) {
    var Freehand = (function () {
        function Freehand() {
            this.points = [];
        }
        Freehand.prototype.AddPoint = function (point) {
            this.points.push(point);
        };
        return Freehand;
    })();
    exports.Freehand = Freehand;
});
```

The differences between an AMD module and a standard JavaScript module are apparent from the very beginning of the definition. The AMD specification provides a function called define that is used to assist the module loader. While there are only two parameters shown for this function here, the function optionally takes a first parameter that precedes the two shown in the generated JavaScript code. This optional first parameter is an identifier for the module, and we will look at this parameter in more detail later in this chapter when we want to minify our final application code. The second parameter for the define function is a list of module dependencies. Every require statement that we use to reference another module in our project will be included in this list. The third and final parameter is the only mandatory parameter for us to define a module, and it is a function that will execute to instantiate an object or module. Each of the dependencies provided to the define function will be provided as parameters to the function for use inside of the module.

The export keyword can also be assigned to a type. In the following example, you can see that rather than decorating our class with the export keyword, we instead use the assignment operator to give it a new value:

```
class Circle implements ICircle {
    constructor(public radius: number) {
    }
    public resize(radius: number) {
        this.radius = radius;
    }
    public area(): number {
        return Math.PI * this.radius * this.radius;
    }
}
export = Circle;
```

Using this method limits us to exporting a single type from a file; however, this type can be a module containing any number of exported objects. The resulting JavaScript looks like the following code:

```
define(["require", "exports"], function(require, exports) {
    var Circle = (function () {
        function Circle(radius) {
            this.radius = radius;
        }
        Circle.prototype.resize = function (radius) {
```
