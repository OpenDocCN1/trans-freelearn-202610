Note that to clear down the data for any reason, repeatedly call the following, then go to your Recycle Bin and delete all org data:
TestData.purgeVolumeData(false);

# # Leveraging Visualforce and the Apex read-only mode

The Start Analysis button calls the service to query the race data analyze the data and for the purposes of this chapter, it outputs the total number of queried rows as follows:

```
```apex
ApexPages.addMessage(
    new ApexPages.Message(
    ApexPages.Severity.Info,
    'Just read ' + Limits.getQueryRows() +
    ' rows from RaceData__c'));
```
```

As per the previous screenshot, select all 10 races (click on the Show 5 more link to show more related list records). Click on the Start Analysis button to display a message, confirming the number of records processed is above the standard 50,000.

![](bbox=[0.26, 0.41, 0.71, 0.52])

The reason this has been made possible is due to the Visualforce page enabling the read-only mode, via the readOnly attribute on the apex:page element, as follows:

```
```xml
<apex:page
    standardController="Race__c"
    extensions="RaceAnalysisController"
    recordSetVar="Races"
    readOnly="true">
```
```

This mode can also be enabled via the @ReadOnly Apex attribute on an Apex scheduled class or on an Apex Remote Action method. As the name suggests, the downside is that there can be no DML statements, so we have no means to update the database. For this use case, this restriction is not a problem. Note that, by using JavaScript Remoting, you can create a mix of methods with and without @ReadOnly. Currently this attribute cannot be used in conjunction with @AuraEnabled.