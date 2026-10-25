```
```typescript
var People = exports.People;
});
```
```

As you can see, the normal module definition is wrapped in another function called `define` that takes an array of dependent objects and a function to be called when all of the dependent modules are loaded. The next thing we need to create is a view model that we can bind our HTML to. We will need observables for the three input values for new people, the value that we want to search against, and an observable array to hold the search results. We will also need a method to initiate the addition process and a method to perform a directory search. We can place all of this code in a separate TypeScript file and use the `import` and `require` keywords to include the People module we just created:

```
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
```

As you can see, we provide the path to the People module to the require function, which tells RequireJS to add the module to the list of modules given to the define method, but also the TypeScript compiler for type information. We define a list of people that will be stored as the full directory list, the new people field observables, the search observables, and the two methods that will be bound to click events. The `findPersonByFirstName` method retrieves the most recent value of the `searchValue` observable as well as the list of people that we will be searching against.