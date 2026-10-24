After successfully adding the method for our service, we will add @AllArgsConstructor and @Service annotations to AntiHeroService. @AllArgsConstructor is an annotation from Lombok that will generate a constructor that requires one argument of each field; in our example, this will produce the following code:

```
public AntiHeroService(AntiHeroRepository repo) {
    this.repo = repo;
}
```

This will now allow the wiring of our AntiHeroRepository dependency on our service.

The @Service annotation, on the other hand, is functional when we want Spring Context to autodetect classes based on their classification.

We have now successfully created our service with CRUD methods, but what if something went wrong as we call our service? For example, the identifier (ID) of the anti-hero we have passed may not exist. We would want to catch the error and return an understandable message to the user. Now, we will create runtime exception handling for our service.

# Runtime exceptions

Runtime exceptions are also known as unchecked exceptions. These are programming errors and will happen upon the current execution of our application. This should be prevented directly by the developers.

Some known runtime exceptions are listed here:

+   - IndexOutOfBoundsException: This exception occurs when we access an index of an array string or an iterable that is out of range, as illustrated in the following code snippet:

```
String[] array = new String[100];
String name = array[200]; // throws index out of bounds
as array variable only has a length of 100
```

+   - IllegalArgumentException: This exception occurs when a method has been passed by an illegal argument, as illustrated in the following code snippet:

```
public class Hero {
    int number;
    public void givePositiveNumber(int number) {
        if(number < 0)  throw new
            IllegalArgumentException(
            Integer.toString(number));
        else { m = number; }
    }
```