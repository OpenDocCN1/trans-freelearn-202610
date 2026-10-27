The useConsoleLog static variable can be changed programmatically at any point of time to give us a chance to change the logging direction. As a result, we don't change the Log class at all.

In our example, the factory constructor is an implementation of the factory method design pattern. It makes a design more customizable and only a little more complicated.

It is always better to use a factory constructor instead of a generative constructor in the following cases:

+   - It is not required to always return a new instance of a class
+   - It is required to instantiate any subtype of the return type
+   - It is essential to reduce the verbosity of creating parameterized type instances of a class

# The singleton design pattern

If we want to keep unique information about a user somewhere in our imaginable framework, then we would have to create a Configuration class for that purpose, as shown in the following code:

```
library configuration;

// Class configuration
class Configuration {
// It always keep our [Configuration]
static final Configuration configuration = new Configuration._();

// Database name
String dbName;

// Private default constructor
Configuration._();
}
```

The Configuration class has a dbName variable to keep the database's name and probably a number of other properties and methods as well. It has a private default constructor, so the class cannot be instantiated from other classes. A static variable configuration is final and will be initialized only once. All looks good and the standards of implementing the singleton design pattern are followed.