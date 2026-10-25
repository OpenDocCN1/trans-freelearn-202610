# Using RequireJS

In Chapter 2, *TypeScript Basics*, we discussed module definitions but at the time we only focused on internal modules. At the time, external modules were mentioned but we did not explore them in-depth. Now that we are familiar with integrating external libraries we will bring in one of the more common external module libraries available. RequireJS is a JavaScript module loader used to ensure that all objects being used during application execution are available when necessary. This will allow us to separate our code into different files and not worry about the order in which they are loaded because RequireJS will manage that for us. Just like jQuery and Knockout, RequireJS and its declaration file are available through the NuGet Package Manager or can be found online at www.requirejs.org. The packages used for this text are “RequireJS” Version 2.1.14 and requires.TypeScript.DefinitelyTyped Version 0.2.0. For this next example, we will need all three libraries available. Let’s put together a simple app that will allow us to add people to a directory and then search that directory.

RequireJS is an **Asynchronous Module Loader (AMD)** so we need to set the appropriate project settings. Open the TypeScript Build section of the project settings and set the Module system option to AMD. Now, we need to create a new module that will contain all of the code related to a set of types that represent people for the directory. Create a new TypeScript file called People.ts in the Scripts folder of the project. As you can see in the following sample, we define the IPerson abstraction and then implement it:

```
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

```
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
```