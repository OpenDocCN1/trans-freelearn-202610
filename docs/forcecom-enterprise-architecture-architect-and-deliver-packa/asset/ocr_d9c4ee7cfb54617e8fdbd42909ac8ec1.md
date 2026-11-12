```
// Given
Display display = new Display();
Dashboard dashboard = new Dashboard(display);
Engine engine = new Engine(dashboard);

// When
Car car = new Car(engine, dashboard);
car.start();

// Then
System.assertEquals(true, car.isRunning());
System.assertEquals(true, engine.isRunning());
System.assertEquals(true, display.isVisible());
System.assertEquals('Hello Driver!',
    display.getMessageShowAt(10, 20));
System.assertEquals('RPM:1000',
    display.getMessageShowAt(10, 10));
}
}
```

> The setup for this test looks simple, but imagine if each of the classes constructed required other configuration and data to be created, the setup logic can grow quickly. This integration test is perfectly valid and useful in its own right. However, as I mentioned earlier, you cannot assume just because you have fully unit tested methods that everything still works when you put it all together!
> 
> The preceding test code is split into three main sections, Given, When, and Then. This type of approach and thinking originates from another practice known as Behavior Driven Development (BDD). You can read more about this from Martin Fowlers web site. https://martinfowler.com/bliki/GivenWhenThen.html

# # Deciding what to test and what not to test for in a Unit test

In this section let's review what type of unit tests we would want to write and what these tests should assert for some of the methods in the preceding example.