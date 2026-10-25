While the `firstName`, `lastName`, and `Age` properties are not observables that can be bound to, the binding will only occur once. If the value of these fields change, the UI will not be updated. The last thing we need to do is create the entry point of our application. We will need to import the view model we created, wait for the DOM to be ready, and apply our bindings:

```
```javascript
import PersonFinderViewModel = require("Scripts/PersonFinderViewModel");
$(document).ready(() => {
    ko.applyBindings(new PersonFinderViewModel.PersonFinderViewModel());
});
```
```

Now we can run the application and add several users to the directory. When we perform a search for someone by their first name, all of the matching results will be displayed:

![](bbox=[0.06, 0.281, 0.536, 0.6])