# # How does Angular enforce It?

What happens if I try to break the separation? What if I try to change the application model inside a setter that is invoked by the change detection system?

Angular tries to make sure that the setter we define for our component only updates the view state of this component or its children and not the application model. To do that Angular will check all bindings twice in the developer mode. First time to propagate changes, and second time to make sure there are no changes. If it finds a change during the second pass, it means that one of our setters updated the application model, the framework will throw an exception, pointing at the place where the violation happened.