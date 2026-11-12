
The following diagram shows how the Domain classes that are used in this chapter map to their respective Custom Objects. Each Domain class receives the records that its methods act on in its constructor. The methods shown in the Domain classes are described in more detail as we progress through this chapter.

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_208_0.png)

## Bulkification

As with the Service layer, code written in the Domain layer is encouraged to think about records in a bulk context. This is important when Domain class methods are being called from an Apex Trigger context and the Service layer code, both of which carry the same bulkification requirement.

Instead of enforcing bulkification through passing parameters and ensuring that parameter types are bulkified, as with the Service layer, the information in the Domain class logic acts on and is driven by its member variables, which are bulkified. In this case, the Records class property returns a `List<SObject>` object:

```
public void onValidate() {
    for(Contestant__c race : (List<Contestant__c>) Records) {
    }
}
```

## Defining and passing data

Most methods on Domain classes already have the information they need through the `Records` property (the state the Domain class was constructed with). As such, there is little need to pass data information on the methods themselves. However, for some custom behavior methods (those not reflecting the Apex Trigger logic), you might want to pass other domain objects and/or a **Unit Of Work** as additional parameters. Examples later in this chapter help illustrate this.

## Transaction management

For Domain class methods that perform database operations, there are two design guidelines: utilize a transaction context (Unit Of Work) passed as a parameter to the method or create one for the scope of the Domain class method execution.

## Domain class template

The Domain class implementation in this chapter utilizes the **FinancialForce.com Apex Enterprise Patterns** library, which is open source and is included in the sample code of this chapter. In this library, the Apex base class, `fflib_SObjectDomain`, is provided to help implement the Domain layer pattern.

A basic template for a Domain class utilizing this base class is shown in the following code snippet:

```
public class Races extends fflib_SObjectDomain {
    public Races(List<Race__c> races) {
        super(races);
    }
}

public class Constructor
    implements fflib_SObjectDomain.IConstructable {
    public fflib_SObjectDomain construct(
        List<SObject> sObjectList) {
        return new Races(sObjectList);
    }
}
```

The first thing to note is that the **constructor** for this class takes a list of `Race__c` records, as per the guidelines described previously. The code implemented in a domain class is written with bulkification in mind. The base class constructor initializes the `Records` base class property.

The inner class, Constructor, is present to permit the dynamic creation of the Domain class in an Apex Trigger context. This is actually working around the lack of full reflection (the ability to reference the class constructor dynamically) in the Apex language. The name of this inner class must always be Constructor.

By extending the fflib_SObjectDomain class, the Domain class inherits properties and methods it can override to implement its own methods, such as the Records property, which provides access to the actual record data. There are virtual methods provided to make the Apex Trigger behavior implementation easier, as well as some specialized events such as onValidate and onApplyDefaults. The following class overview illustrates some of the key methods in the base class that the Domain classes can override:

```
fflib_SObjectDomain(List<SObject>, SObjectType)
v onApplyDefaults() : void
v onValidate() : void
v onValidate(Map<Id, SObject>) : void
v onBeforeInsert() : void
v onBeforeUpdate(Map<Id, SObject>) : void
v onBeforeDelete() : void
v onAfterInsert() : void
v onAfterUpdate(Map<Id, SObject>) : void
v onAfterDelete() : void
v onAfterUndelete() : void
v handleBeforeInsert() : void
v handleBeforeUpdate(Map<Id, SObject>) : void
v handleBeforeDelete() : void
v handleAfterInsert() : void
v handleAfterUpdate(Map<Id, SObject>) : void
v handleAfterDelete() : void
v handleAfterUndelete() : void
```

## Implementing Domain Trigger logic

The most common initial use case for a Domain class is to encapsulate the Apex Trigger logic. In order to enable this, a small Apex Trigger is required to invoke the triggerHandler method. This will route the various Trigger events to the appropriate methods in the Domain class (as shown in the preceding screenshot), avoiding the need for the usual if/else logic around Trigger.isXXX variables.

The name of this trigger can be anything, though it makes sense to match it with that of the corresponding Domain class. Once this is in place, you can ignore it and focus on implementing the Domain class methods as follows:

```
trigger Seasons on Season__c (
  after delete, after insert, after update,
  before delete, before insert, before update) {
    fflib_SObjectDomain.triggerHandler(Seasons.class);
}
```

## Routing trigger events to Domain class methods

The following diagram illustrates the flow of execution from the Apex Trigger to the Domain class, triggerHandler, to the individual methods:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_211_0.png)

To help you understand how traditional Apex Trigger events are mapped to the various virtual methods in the base class, this table describes the code path taken from when the base class's fflib_SObjectDomain.triggerHandler method is invoked through the specific event-based methods that can be overridden by a Domain class:

| Trigger context | Base class Records property value | Base class handle method called (refer to the following note) | Base class event method(s) called (refer to the following note) |
| :--- | :--- | :--- | :--- |
| Trigger.isBefore | | | |
| Trigger.isInsert | Trigger.new | handleBeforeInsert() | onApplyDefaults()<br>onBeforeInsert() |
| Trigger.isUpdate | Trigger.new | handleBeforeUpdate(Map<Id, SObject> existingRecords) | onBeforeUpdate(Map<Id, SObject> existingRecords) |
| Trigger.isDelete | Trigger.oldMap | handleBeforeDelete() | onBeforeDelete() |
| Trigger.isAfter | | | |
| Trigger.isInsert | Trigger.new | handleAfterInsert() | onValidate()<br>onAfterInsert() |
| Trigger.isUpdate | Trigger.new | handleAfterUpdate(Map<Id, SObject> existingRecords) | onValidate(Map<Id, SObject> existingRecords)<br>onAfterUpdate(Map<Id, SObject> existingRecords) |
| Trigger.isDelete | Trigger.oldMap | handleAfterDelete() | onAfterDelete() |

Note: Handle methods can also be overridden if you wish to implement your own handling. The existingRecords parameter represents the value of Trigger.oldMap.

The base class uses the template method pattern (http://en.wikipedia.org/wiki/Template_method_pattern). This ensures that when overriding the onXXX methods from the base class, you do not need to worry about calling the base class version of the method, as is the typical developer requirement when overriding methods in OOP.

## Enforcing object security

Salesforce requires developers to implement the Object CRUD (Create, Read, Update, and Delete) security and Field Level security checks using the methods on the SObjectDescribe and SObjectFieldDescribe Apex runtime types.

By default, the preceding handle methods in the fflib_SObjectDomain base class used in this chapter automatically perform the Object CRUD security on behalf of the developers Domain class logic.

Note that currently the base class still leaves implementing Field Level security to the developer to implement.

A developer controlled configuration option provided by the base class allows each Domain class to control whether this default behavior is enabled or not. For example, objects that are typically maintained by the application code on behalf of the user may not wish to have this check enforced, such as the awarding of championship points. This type of access is sometimes referred as system level access, as the system performs an operation on behalf of the user. Thus by accepting the default to enforce this check, it would require users to be granted access which would not be appropriate elsewhere in the application. Another motivation for not leveraging the default enforcement would be a preference to code these checks in your controller logic more explicitly. Consider carefully the default enforcement is what you need in each case.

## Default behavior

The handler method checks the required security before calling the preceding event methods and throws a DomainException exception if the user does not have the required access (forcing the entire trigger context to stop). For example, when the handleAfterInsert method is invoked, the SObjectDescribe.isCreatable method is used.
