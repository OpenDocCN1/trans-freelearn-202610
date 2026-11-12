The name of this trigger can be anything, though it makes sense to match it with that
of the corresponding Domain class. Once this is in place, you can ignore it and focus
on implementing the Domain class methods as follows:

```
trigger Seasons on Season__c (
  after delete, after insert, after update,
  before delete, before insert, before update) {
    fflib_SObjectDomain.triggerHandler(Seasons.class);
}
```

# Routing trigger events to Domain class
methods

The following diagram illustrates the flow of execution from the Apex Trigger to the
Domain class, triggerHandler, to the individual methods:

![](bbox=[0.2, 0.379, 0.85, 0.661])