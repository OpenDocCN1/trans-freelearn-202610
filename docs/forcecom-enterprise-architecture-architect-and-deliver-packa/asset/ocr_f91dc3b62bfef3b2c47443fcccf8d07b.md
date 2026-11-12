Formula fields can be indexed so long as the formula result is deterministic, which means that it does not use any dynamic data or time functions such as TODAY or NOW, or leverage-related fields via lookup fields or any other formula fields that are non deterministic.

As creating indexes and maintaining them consumes platform resources, custom indexes cannot be packaged, as the decision to enable them is determined by conditions only found in the subscriber org.

Though objects can contain millions of records, standard indexes will only index up to 1 million records and custom indexes up to 333,333 records. These limits have a bearing on whether an index is used or not, depending on the query and filter criteria being used. This is discussed in more detail further in this chapter.

With respect to the Race Data object, the following fields will be indexed by default by the platform. Only through contacting Salesforce support and enabling in the subscriber org can additional custom indexes be created, for example, on the Type__c field.

| Field | Standard Index |
|---|---|
| Created By | Yes (Lookup Field) |
| LastModifiedBy | Yes (Lookup Field) |
| Name | Yes |
| Owner | Yes |
| Contestant__c | Yes (Lookup Field) |
| Race__c | Yes (Lookup Field) |
| DriverId__c | No |
| Lap__c | No |
| RaceName__c | No |
| Sector__c | No |
| Type__c | No |
| Value__c | No |
| Year__c | No |

Under Setup, navigate to Objects and locate the Race Data object. When viewing fields listed in the Standard Fields or Custom Fields & Relationships related lists, observe the Index column. This is the easiest way to determine whether a field has a standard or custom index applied to it.