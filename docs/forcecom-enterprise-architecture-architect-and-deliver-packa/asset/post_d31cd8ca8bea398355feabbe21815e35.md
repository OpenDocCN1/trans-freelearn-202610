
## Overriding the default behavior

To make it easier to configure the fflib_SObjectDomain class throughout the application, as well as to provide a means to establish shared or common Domain logic code, a new base class is created as follows.

```
/**
 * Application specific Domain base class,
 * customise fflib_SObjectDomain and add common behavior
 **/
public abstract class ApplicationDomain extends
	fflib_SObjectDomain {
	public ApplicationDomain(List<SObject> records) {
		super(records);
		// Disable CRUD security enforcement at the Domain class level
		Configuration.disableTriggerCRUDSecurity();
	}
}
```

Thus Domain classes in this application extended this class instead.

```
public class Contestants extends ApplicationDomain {
	public Contestants(List<Contestant__c> contestants) {
		super(contestants);
	}
}
```

Note that this approach requires that the developer takes over exclusive responsibility for implementing Object CRUD security checking them.

## Apex Trigger event handling

The upcoming sections illustrate some Apex Trigger examples with respect to the FormulaForce application. They utilize the Drivers and Contestants Domain classes included in the sample code for this chapter along with the following new fields:

- In the Driver__c object, a new field called ShortName__c as a Text type, with a maximum size of 3 characters, has been created.
- In the Race__c object, there is a new field called Status__c as a Picklist type, with the Scheduled, In Progress, Red Flagged, and Finished values. The default value is scheduled by ticking the Use first value as default value checkbox.

## Defaulting field values on insert

Although the onInsertBefore method can be overridden to implement logic to set default values on record insert, the following code, added to the Contestants Domain class, has chosen to override the onApplyDefaults method instead. This was used as it is more explicit and permits a Separation of Concerns between the defaulting code and the record insert code, although both are fired during the insert phase:

```
public override void onApplyDefaults() {
    for(Driver__c driver : (List<Driver__c>) Records) {
        if(driver.ShortName__c == null) {
            // Upper case first three letters of drivers last name
            String lastName = driver.Name.substringAfterLast(' ');
            driver.ShortName__c = lastName.left(3).toUpperCase();
        }
    }
}
```

The preceding code attempts to populate the Drivers short name, which is typically made up of the first three characters of their second name in upper case, so Lewis Hamilton will become HAM.

## Validation on insert

The following validation logic applies during record creation. It has been added to the Contestants Domain class and ensures that contestants are only added when the race is in the Scheduled state.

It also demonstrates the use of a Selector pattern class to bulk load records from the Race__c Custom Object, and this pattern will be covered in detail in the next chapter:

```
public override void onValidate() {
    // Bulk load the associated races
    Set<Id> raceIds = new Set<Id>();
    for(Contestant__c contestant : (List<Contestant__c>) Records) {
        raceIds.add(contestant.Race__c);
    }
    Map<Id, Race__c> associatedRaces =
        new Map<Id, Race__c>(
        new RacesSelector().selectById(raceIds));
    // Only new contestants to be added to Scheduled races
    for(Contestant__c contestant :
        (List<Contestant__c>) Records) {
        Race__c race = associatedRaces.get(contestant.Race__c);
        if(race.Status__c != 'Scheduled') {
            contestant.addError(
            'Contestants can only be added to scheduled races');
        }
    }
}
```

By overriding the onValidate method, the logic always invokes the after phase of the Apex Trigger. As no further edits can be made to the record fields, this is the safest place to perform the logic. Always keep in mind that your Apex Trigger might not be the only one assigned to the object. For example, once your package is installed in a subscriber org, a subscriber-created custom trigger on your object might fire before yours (the order of trigger invocation is non-deterministic) and will attempt to modify fields. Thus, for managed package triggers, it is highly recommended that validation be performed in the after phase to ensure complete confidence and security, as the data will not be modified further after validation.

## Validation on update

An alternative method, override, is provided to implement the validation logic applicable to record updates. It is an overload of the onValidate method shown in the preceding example, used to pass in the current records on the database for comparison if needed:

```
public override void onValidate(
    Map<Id, SObject> existingRecords) {
    // Bulk load the associated races
    Map<Id, Race__c> associatedRaces = queryAssociatedRaces();
    // Can only change drivers in scheduled races
    for(Contestant__c contestant : (List<Contestant__c>) Records) {
        Race__c contestantRace =
            associatedRaces.get(contestant.Race__c);
        Contestant__c oldContestant =
            (Contestant__c) existingRecords.get(contestant.Id);
        if(contestantRace.Status__c != 'Scheduled' &&
            contestant.Driver__c != oldContestant.Driver__c) {
            contestant.Driver__c.addError(
            'You can only change drivers for scheduled races');
        }
    }
}
```

The preceding code leverages a new custom Domain class method to share the code responsible for loading associated **Race** records for **Contestants** across the onValidate method shown in the previous example. This method also uses the Records property to determine **Contestants** in scope:

```
private Map<Id, Race__c> queryAssociatedRaces() {
    // Bulk load the associated races
    Set<Id> raceIds = new Set<Id>();
    for(Contestant__c contestant : (List<Contestant__c>) Records) {
        raceIds.add(contestant.Race__c);
    }
    return new Map<Id, Race__c>(
        new RacesSelector().selectById(raceIds)
    );
}
```

> The preceding method is marked as private, as there is currently no need to expose this functionality to other classes. Initially, restricting methods this way is often considered best practice and permits easier internal refactoring or improvements in the future. If a need arises that needs access to this functionality, the method can be made public.

## Implementing custom Domain logic

A Domain class should not restrict itself to containing logic purely related to Apex Triggers. In the following example, the code introduced in the previous chapter to calculate championship points has been refactored into the Contestants Domain class. This is a more appropriate place for it, as it directly applies to the Contestant record data and can be readily shared between other Domain and Service layer code (which we will look at later in this chapter):

```
public void awardChampionshipPoints(fflib_ISObjectUnitOfWork uow) {
    // Apply championship points to given contestants
    Map<Integer, ChampionshipPoint__mdt> pointsByTrackPosition =
        new ChampionshipPointsSelector().selectAllByTrackPosition();
    for(Contestant__c contestant : (List<Contestant__c>) Records) {
        // Determine points to award for the given position
        ChampionshipPoint__mdt pointsForPosition =
            pointsByTrackPosition.get(
            Integer.valueOf(contestant.RacePosition__c));
        if (pointsForPosition != null) {
            // Apply points and register for update with uow
            contestant.ChampionshipPoints__c =
                pointsForPosition.PointsAwarded__c;
            uow.registerDirty(contestant);
        }
    }
}
```

Note that the **Unit Of Work** is passed as a parameter here so that the method can register work (record updates) with the calling Service layer method responsible for committing the work. Once again, as with the previous Domain class methods, the `Records` property is used to access the **Contestant** records to process.

## Object-oriented programming

One of the big advantages of using Apex classes is the ability to leverage the power of OOP. OOP allows you to observe commonalities in data or behavior across your objects to share code or apply common processing across different objects.

An example of such a commonality in the Formula1 world are rules; every aspect of the sport has a set of rules and regulations to comply with, such as **drivers** owning a FIA **Super License**, the weight of the car they drive should be at least above a defined minimum, and ensuring that a **team** has not exceeded the maximum distance in testing their cars. Such compliances are checked regularly, both before and after a race.

## Creating a compliance application framework

In our FormulaForce application, we want to create a compliance framework that will check these regulations across the different objects while also providing a consistent user interface experience for the end user to verify compliance. The initial requirement is to place a **Verify Compliance** button on the **Detail Pages** on all applicable objects.

The **Visualforce Controller** and **Service code** that perform the verification process should be generic and reusable across these objects, while the actual compliance-checking logic associated with each object will be specific. By using an **Apex Interface**, we can define this common requirement for the applicable Domain classes to implement.

In this section, we will implement this requirement using an Apex interface and a generic service that can be passed record IDs for different Domain objects across the application. Creating such a service avoids code duplication by having to repeat the logic of handling the results within the code of each Domain class and provides a centralized service for the controller to consume regardless of the object type in scope.
