# # Introducing the Selector factory

The last chapter introduced the concept of a **Domain factory**, which was used to dynamically construct Domain class instances implementing a common **Apex Interface** in order to implement the compliance framework.

The following code is used in the ComplianceService.verify method's implementation, making no reference at all to a Selector class to query the records needed to construct the applicable Domain class:

```
fflib_SObjectDomain domain =
Application.Domain.newInstance(recordIds);
```

So, how did the Domain factory retrieve the records in order to pass them to the underlying Domain class constructor? The answer is that it internally used another factory implementation called the **Selector factory**.

As with the Domain factory, the Selector factory resides within the Application class as a static instance, exposed via the Selector static class member, as follows:

```
public class Application
{
    // Configure and create the SelectorFactory for this Application
    public static final fflib_Application.SelectorFactory Selector =
        new fflib_Application.SelectorFactory(
            new Map<SObjectType, Type> {
                Team__c.SObjectType =>TeamsSelector.class,
                Race__c.SObjectType =>RacesSelector.class,
                Car__c.SObjectType =>CarsSelector.class,
                Driver__c.SObjectType =>DriversSelector.class,
                Contestant__c.SObjectType => ContestantsSelector.class }});

    // Configure and create the DomainFactory for this Application
    public static final fflib_Application.DomainFactory Domain =
        new fflib_Application.DomainFactory(
            Application.Selector,
            // Map SObjectType to Domain Class Constructors
            new Map<SObjectType, Type> {
                Team__c.SObjectType =>Teams.Constructor.class,
                Race__c.SObjectType =>Races.Constructor.class,
                Car__c.SObjectType =>Cars.Constructor.class,
                Driver__c.SObjectType =>Drivers.Constructor.class,
                Contestant__c.SObjectType =>
                Contestants.Constructor.class}});
}
```