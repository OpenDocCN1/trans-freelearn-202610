```
if(race.Status_c != 'Scheduled') {
    contestant.addError(
    'Contestants can only be added to scheduled races');
    }
}
```

By overriding the onValidate method, the logic always invokes the after phase of the Apex Trigger. As no further edits can be made to the record fields, this is the safest place to perform the logic. Always keep in mind that your Apex Trigger might not be the only one assigned to the object. For example, once your package is installed in a subscriber org, a subscriber-created custom trigger on your object might fire before yours (the order of trigger invocation is non-deterministic) and will attempt to modify fields. Thus, for managed package triggers, it is highly recommended that validation be performed in the after phase to ensure complete confidence and security, as the data will not be modified further after validation.

# Validation on update

An alternative method, override, is provided to implement the validation logic applicable to record updates. It is an overload of the onValidate method shown in the preceding example, used to pass in the current records on the database for comparison if needed:

```
public override void onValidate(
    Map<Id,SObject>existingRecords) {
    // Bulk load the associated races
    Map<Id, Race__c>associatedRaces = queryAssociatedRaces();
    // Can only change drivers in scheduled races
    for(Contestant__c contestant : (List<Contestant__c>) Records) {
    Race__ccontestantRace =
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