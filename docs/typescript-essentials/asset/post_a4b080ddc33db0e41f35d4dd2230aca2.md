
![](img/83a7f7e1723158e56008cf70304a8da4_188_0.png)

As you can see, we have added the script tag for jQuery to the document’s head tag, and a couple of elements to the body. The first element is a large square div with a blue background. The second is a button that the user will click to hide the blue div tag that is shown. Now that we have the objects we want to manipulate, let’s create some TypeScript to perform the expected operation.

In order to use jQuery in a strongly typed manner with the TypeScript compiler, we must include a reference to the declaration file at the top of the TypeScript file we wish to use it in. This is done using a specific format at the top of the TypeScript file; placing references anywhere but at the top of the page will cause them not to function. In the following example, you can see how to reference declaration files:

```typescript
/// <reference path="scripts/typings/jquery/jquery.d.ts" />
```

The path included in the reference is relative to the file you are currently working in. Once this reference has been added, we are able to use IntelliSense and code completion to help us work with the jQuery API.

![](img/83a7f7e1723158e56008cf70304a8da4_189_0.png)

As you can see, we get the full feature set associated with Visual Studio’s code completion.

### Tip

When integrating with a large number of libraries it can be easier to manage your references by creating a `_references.d.ts` file and adding references only to this file. Then, each of your code files only has to reference the single `_references.d.ts` file to import all of the relevant type information for the project.

As you can see in the following code sample, locating objects on the DOM and manipulating them is far easier with jQuery included and will reduce the amount of code we have to produce to make a dynamic application:

```typescript
/// <reference path="scripts/typings/jquery/jquery.d.ts" />
$(document).ready(() => {
    $("#hideButton").click((event) => {
        $("#colorPanel").hide(1000);
    });
});
```

The first element in the file is the reference to the jQuery declaration file. This is what allows us to use the $ notation in our TypeScript without generating a compiler error, however, jQuery still needs to be loaded for this to work at runtime. We wait for the DOM to finish loading, which causes the function delegate to be called. At this time, we select the button we added to our HTML earlier on and listen for its click event to fire. When this occurs, another function delegate is called, which uses one of jQuery’s UI modification methods called hide to make the large square div tag disappear over a period of 1000 milliseconds. Now if the application is run, there will be a large blue square just above the hide button. Once the button is clicked, you can watch the blue box disappear and the UI adjust for its disappearance. jQuery has a very large set of features and extensions. To learn more about this I recommend visiting [http://jquery.com/](http://jquery.com/) for an API reference and samples.

![](img/83a7f7e1723158e56008cf70304a8da4_191_0.png)

As you can see, we include the jQuery and Knockout libraries in the head. Then, in the body of the HTML, we have the two elements we created during the jQuery example. However, each of these elements now contains bindings. The `colorPanel` div is bound to the `IsVisible` observable using Knockout’s visible binding. When the IsVisible observable changes, Knockout will evaluate the new value and manipulate the DOM to show or hide the div. The button is bound to multiple elements on the view model, the `toggleText` object, which will determine what the value of the button is, and the `toggleClick` method, which will execute anytime the click event fires.

### Tip

Knockout supports many different kinds of bindings and also has a custom binding model that will allow you to define your own bindings. Visit [www.knockoutjs.com](http://www.knockoutjs.com) to learn more.

OK, we have most of the pieces together now to dynamically bind the DOM to our TypeScript/JavaScript but we need to actually connect the view and the view model. The following code segment shows how to bind the two together:

```javascript
$(document).ready(() => {
    ko.applyBindings(new ViewModel());
});
```

As you can see, we wait for the DOM to be ready before we apply the binding. If we attempt to bind before all of the elements are available an error will occur at runtime. Once the DOM is ready, we create a new instance of our `ViewModel` class and pass it to Knockout, which will handle the rest. Optionally, you can pass in a specific element to bind the view model to. Now when we run the application it starts with the large blue square visible and the text of the button is **Hide**. However, instead of this being the end of the application’s life cycle as it was earlier, the button text has changed and we can perform another action, as shown in the following screenshot:

![](img/83a7f7e1723158e56008cf70304a8da4_192_0.png)

## Using RequireJS

In Chapter 2, *TypeScript Basics*, we discussed module definitions but at the time we only focused on internal modules. At that time, external modules were mentioned but we did not explore them in-depth. Now that we are familiar with integrating external libraries we will bring in one of the more common external module libraries available. RequireJS is a JavaScript module loader used to ensure that all objects being used during application execution are available when necessary. This will allow us to separate our code into different files and not worry about the order in which they are loaded because RequireJS will manage that for us. Just like jQuery and Knockout, RequireJS and its declaration file are available through the NuGet Package Manager or can be found online at www.requirejs.org. The packages used for this text are “RequireJS” Version 2.1.14 and RequireJS.TypeScript.DefinitelyTyped Version 0.2.0. For this next example, we will need all three libraries available. Let’s put together a simple app that will allow us to add people to a directory and then search that directory.

RequireJS is an **Asynchronous Module Loader (AMD)** so we need to set the appropriate project settings. Open the TypeScript Build section of the project settings and set the Module system option to AMD. Now, we need to create a new module that will contain all of the code related to a set of types that represent people for the directory. Create a new TypeScript file called People.ts in the Scripts folder of the project. As you can see in the following sample, we define the IPerson abstraction and then implement it:

```typescript
export interface IPerson {
    firstName: string;
    lastName: string;
    age: number;
}
export class Person implements IPerson {
    constructor(public firstName: string, public lastName: string, public age: number) {
    }
}
```

Both the IPerson interface and Person class are decorated with the export keyword, which will tell the compiler that these types are available in other modules that use this module. The resulting JavaScript is a little different from what was generated for internal modules:

```javascript
define(["require", "exports"], function(require, exports) {
    (function (People) {
        var Person = (function () {
            function Person(firstName, lastName, age) {
                this.firstName = firstName;
                this.lastName = lastName;
                this.age = age;
            }
            return Person;
        })();
        People.Person = Person;
    })(exports.People || (exports.People = {}));
    var People = exports.People;
});
```

As you can see, the normal module definition is wrapped in another function called `define` that takes an array of dependent objects and a function to be called when all of the dependent modules are loaded. The next thing we need to create is a view model that we can bind our HTML to. We will need observables for the three input values for new people, the value that we want to search against, and an observable array to hold the search results. We will also need a method to initiate the addition process and a method to perform a directory search. We can place all of this code in a separate TypeScript file and use the `import` and `require` keywords to include the People module we just created:

```typescript
import People = require("Scripts/People");
export class PersonFinderViewModel {
    public peopleArray: KnockoutObservableArray<People.IPerson> = ko.observableArray([]);
    public newFirstName: KnockoutObservable<string> = ko.observable("");
    public newLastName: KnockoutObservable<string> = ko.observable("");
    public newAge: KnockoutObservable<number> = ko.observable(null);
    public searchValue: KnockoutObservable<string> = ko.observable("");
    public searchResult: KnockoutObservableArray<People.IPerson> = ko.observableArray([]);
    constructor() {
    }
    public findPersonByFirstName(viewModel: PersonFinderViewModel, event: JQueryEventObject) {
        var people = this.peopleArray();
        var searchValue = this.searchValue();
        this.searchResult([]);
        for (var i = 0; i < people.length; i++) {
            if (searchValue === people[i].firstName) {
                this.searchResult.push(people[i]);
            }
        }
    }
    public addNewPerson(viewModel: PersonFinderViewModel, event: JQueryEventObject) {
        var newPerson = new People.Person(this.newFirstName(), this.newLastName(), this.newAge());
        this.peopleArray.push(newPerson);
        this.newFirstName("");
        this.newLastName("");
        this.newAge(null);
    }
}
```

As you can see, we provide the path to the People module to the require function, which tells RequireJS to add the module to the list of modules given to the define method, but also the TypeScript compiler for type information. We define a list of people that will be stored as the full directory list, the new people field observables, the search observables, and the two methods that will be bound to click events. The `findPersonByFirstName` method retrieves the most recent value of the `searchValue` observable as well as the list of people that we will be searching against.
