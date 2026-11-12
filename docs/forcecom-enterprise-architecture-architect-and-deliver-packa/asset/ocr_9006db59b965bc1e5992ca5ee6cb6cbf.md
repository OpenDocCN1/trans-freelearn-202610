The following components have been added to the application to support this example:

+   - A new **Team** object and tab
+   - A new **Testing Distance** number field with a length of 6 on the **Team** object
+   - A new **Team** lookup field on the **Driver** object
+   - A new Teams Domain class

> You don't always have to create an Apex Trigger that calls the `fflib_SObjectDomain.triggerHandler` method if you don't plan on overriding any of the Apex Trigger event handler methods.

Being able to call between the Domain layer classes permits the driver compliance-checking code to continue to be reused at the **Driver** level as well as from the **Team** level. This Domain class example shows the Teams Domain class calling the Drivers Domain class:

```
public class Teams extends fflib_SObjectDomain
implements ComplianceService.ICompliant {
  public List<ComplianceService.VerifyResult> verifyCompliance() {
    // Verify Team compliance
    List<ComplianceService.VerifyResult>teamVerifyResults =
    new List<ComplianceService.VerifyResult>();
    for(Team__c team : (List<Team__c>) Records) {
    ComplianceService.VerifyResulttestingDistance =
    new ComplianceService.VerifyResult();
    testingDistance.ComplianceCode = '22.5';
    testingDistance.RecordId = team.Id;
    testingDistance.passed = team.TestingDistance__c!=null ?
    team.TestingDistance__c <= 15000 : true;
    testingDistance.failureReason = testingDistance.passed ?
    null : 'Testing exceeded 15,000km';
    teamVerifyResults.add(testingDistance);
    }

    // Verify associated Drivers compliance
    teamVerifyResults.addAll(
    new Drivers(
    new DriversSelector().selectDriversByTeam(
    new Map<Id, SObject>(Records).keySet()))
    .verifyCompliance());
  }
}
```