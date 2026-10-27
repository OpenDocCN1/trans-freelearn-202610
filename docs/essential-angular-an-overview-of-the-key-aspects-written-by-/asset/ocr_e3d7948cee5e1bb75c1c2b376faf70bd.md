# Let's recap

That's how Angular templates work. Let's see if they are analyzable and transformable, and if they provide the benefits I talked about at the beginning of this chapter.

Contrasting with Angular 1, there is a lot more the framework can tell about the template statically.

For example, regardless of what the component element is, Angular knows that `name in [property1]="name"` is a field read and `name in property2="name"` is a string literal. It also knows that the `name` property cannot be updated by the component: property bindings update from the parent to the child. Similarly, Angular can tell what variables are defined in the template by just looking at `*ngFor`. Finally, it can tell apart the static and dynamic parts in the template.

All of these allow us to reliably introspect and transform templates at compile time. That's a powerful feature, that, for instance, the Angular i18n support is built upon.

Having designed templates in this way, we also solved one of the biggest Angular 1 problems: IDE support. When using Angular 1, editors and IDEs had to do a lot of guessing to provide completion, refactoring, and navigation....