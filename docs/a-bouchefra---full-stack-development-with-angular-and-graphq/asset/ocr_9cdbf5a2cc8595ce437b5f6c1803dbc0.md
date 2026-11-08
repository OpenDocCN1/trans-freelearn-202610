```
title = 'client';
}
```

This is just an ordinary TypeScript class that has the `@Component` decorator, imported from the core module, applied to it. The decorator contains metadata about the component, such as the following:

+   - `selector`: This is used in the same way as conventional HTML tags to call the component from an HTML page. You may use Angular to build custom HTML tags that are only useable within your application.
+   - `templateUrl`: This is used to tell Angular where to look for the HTML template that contains the presentation markup.
+   - `styleUrls`: This is used to designate where to find component-specific styles that determine the appearance and feel of the component.

A component's life cycle begins when Angular constructs an instance of the component's class and ends when the component is destroyed and removed from the DOM.

Angular supports lifecycle hook methods via a slew of interfaces, each of which contains a method, to allow you to hook onto important events in a component's lifecycle.

The `OnInit` interface, for example, provides the `ngOnInit` method, which is run when the component is initialized. Please refer to https://angular.io/guide/glossary#lifecycle-hook for further information about lifecycle hooks.

Now that we've defined modules and components, let's look at services and dependency injection.

# Understanding services and dependency injection

A service in Angular is a singleton that may be provided to other components or services using `dependency injection`, which is a way to supply dependencies to an object.

Don't be misled by this terminology! This simply means that an Angular entity called the **injector** is in charge of creating an instance of the service and delivering it to the components that request it.

Dependency injection is tightly linked with the Angular framework and is used to inject services and other objects into components, allowing access to the service functionality without having to manually create an instance.