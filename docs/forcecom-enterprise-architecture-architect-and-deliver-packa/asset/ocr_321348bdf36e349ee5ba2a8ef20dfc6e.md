```
racesQueryFactory.setCondition('Id in :raceIds').toSOQL();
}
```

The preceding code uses the fflib_SObjectSelector method addQueryFactorySubselect. This method creates a new fflib_QueryFactory instance and passes it on to the query factory passed into the method, in this case, the racesQueryFactory. When the toSOQL method is called on the outer query factory it automatically generates sub-queries for any contained query factories.

By reusing an instance of the ContestantsSelector, it is ensured that no matter how a list of Contestant__c records are queried (directly or as part of a sub-select query), the fields are populated consistently. The getOrderBy method on ContestantSelector is also used to ensure that even though the records are being returned as part of a sub-select, the default ordering of Contestant records is also maintained.

Let's take another look at the example used earlier to illustrate how this pattern has avoided the dangers of inconsistent querying. Although the Contestant__c records are queried from two different selectors, RacesSelector and ContestantsSelector, the result in terms of fields populated is the same due to the reuse of the ContestantsSelector class in the preceding implementation.

The following example shows the standard selectById method being used to query Contestants to pass to the Contestants Domain class constructor, as part of the Service layer implementation to award championship points:

```
List<Contestant__c> contestants =
    new ContestantsSelector().selectById(contestantIds);
new Contestants(contestants).awardChampionshipPoints(uow);
```

The following alternative example shows the custom selectByIdWithContestants method on RaceSelector being used to query Races and Contestants to pass to the Contestants Domain class constructor to achieve the same result:

```
List<Contestant__c> contestants = new List<Contestant__c>();
for(Race__c race :
    new RacesSelector().selectByIdWithContestants(raceIds)) {
    contestants.addAll(race.Contestants__r);
}
new Contestants(contestants).awardChampionshipPoints(uow);
```

In either of these two examples, the fields and ordering of the records are consistent, despite the way in which they have been queried, either as a direct query or sub-select. This gives the developer more confidence in using the most optimum approach to query the records needed at the time.