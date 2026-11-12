```
}
static class RequestFactoryProvider implements
Provider<ApplicationRequestFactory> {
    
}
    
}
}
```

The Roo-generated ScaffoldModule class extends AbstractGinModule and overrides the configure method to associate dependencies with their providers. The bind method of AbstractGinModule binds a dependency to its provider. The in(Singleton.class) instructs that only a single instance of EventBus, PlaceController, and ApplicationRequestFactory are created for the application. The PlaceControllerProvider and RequestFactoryProvider static inner classes represent provider for the PlaceController and RequestFactory instances, respectively. SimpleEventBus (part of GWT API) is an implementation of EventBus.

An important point to notice in this code is the use of @Inject annotation by PlaceControllerProvider for instructing Ginjector to inject an implementation of EventBus. So, if you are creating a hierarchy of objects, then define the method in Ginjector, which returns the top-level object because the lower-level objects in the hierarchy can make use of Ginjector's dependency injection feature using the @Inject annotation.

The following figure summarizes how the Scaffold class makes use of the GIN framework to bootstrap the application:

![](bbox=[0.193, 0.542, 0.81, 0.752])

The Scaffold class' onModuleLoad method invokes the getInjector method of DesktopInjectWrapper to access Ginjector implementation, followed by a call to getScaffoldApp to obtain an instance of ScaffoldDesktopApp. The following sequence diagram further clarifies the sequence of method invocations between classes: