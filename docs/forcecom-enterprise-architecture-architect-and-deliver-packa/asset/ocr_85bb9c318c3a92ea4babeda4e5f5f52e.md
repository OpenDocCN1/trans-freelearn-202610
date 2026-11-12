# SOQL | Resulting plans

select id from
RaceData__c where Type__c
= 'Sector Time' and
RaceName__c = 'Spa'

Again, a table scan was selected even though the query was made more selective by adding the race name. Adding a custom index over the race name would be advised here, as it is likely to be a common filter criteria.

```
{
    "plans" : [ {
        "cardinality" : 1000,
        "fields" : [ ],
        "leadingOperationType" :
        "TableScan",
        "relativeCost" : 0.68833333333333334,
        "sobjectCardinality" : 100000,
        "sobjectType" : "fforce__RaceData__c"
    }, {
        "cardinality" : 16640,
        "fields" : [ "fforce__Type__c" ],
        "leadingOperationType" : "Index",
        "relativeCost" : 1.664,
        "sobjectCardinality" : 100000,
        "sobjectType" : "fforce__RaceData__c"
    } ]
}
```

select id from
RaceData__c where Type__c
!= 'PitStop Time'

A table scan plan was selected, as a negative operator was used.

```
{
    "plans" : [ {
        "cardinality" : 100000,
        "fields" : [ ],
        "leadingOperationType" :
        "TableScan",
        "relativeCost" : 2.8333333333333335,
        "sobjectCardinality" : 100000,
        "sobjectType" : "RaceData__c"
    } ]
}
```