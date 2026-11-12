
> The fflib_QueryFactory class methods provide an alternative to using the standard string concatenation or String.format approaches when building dynamic SOQL queries. The original author of the fflib_QueryFactory class, Chris Peterson, was motivated in creating it to avoid the fragility that string-based methods can give as well as to provide a more readable means of understanding what such code was doing. In addition because the class supports using SObjectField references, as opposed to field name references in strings, such code is less prone to SOQL-injection vulnerabilities.

## A basic custom Selector method

Let's look first at a basic example, which queries Driver__c based on the team they are assigned to. As per the guidelines, the parameters are bulkified and allow you to query for drivers across multiple teams if needed.

```
public List<Driver__c> selectByTeam(Set<Id> teamIds) {
    return (List<Driver__c>)
    Database.query(
    newQueryFactory().
    setCondition('Team__c in :teamIds').
    toSOQL();
    }
```

By using the newQueryFactory method, the fflib_QueryFactory instance is already populated with knowledge of the field list, object name, and order by clause specified by the selector. Thus this Selector method behaves consistently in terms of the fields populated and the order of the records returned.

## A custom Selector method with subselect

In the following example, the querying being made uses a sub-select to also include child records (in this case, Contestants within Race). The key thing here is that the RacesSelector method (shown in the following code) reuses an instance of the ContestantsSelector class to access its field list and perform security checking:

```
public List<Race__c> selectByIdWithContestants(Set<Id> raceIds) {
    fflib_QueryFactory racesQueryFactory = newQueryFactory();
    fflib_QueryFactory contestantsSubQueryFactory =
    new ContestantsSelector().
    addQueryFactorySubselect(racesQueryFactory);
    return (List<Race__c>) Database.query(
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

The selectByIdWithContestants method generates the following SOQL:

```
select
Name, TotalDNFs__c, Status__c, Season__c, Id,
PollPositionLapTime__c, FastestLapBy__c,
(select
Qualification1LapTime__c, ChampionshipPoints__c,
Driver__c, GridPosition__c, Qualification3LapTime__c,
RacePosition__c, Name, DNF__c, RaceTime__c,
Id, DriverRace__c, Qualification2LapTime__c, Race__c
from Contestants__r
order by Race__r.Season__r.Name, Race__r.Name,
RacePosition__c)
from Race__c where id in :raceIds order by Name
```

## A custom Selector method with related fields

In addition to querying child records, related records can also be queried using the SOQL field dot notation. This provides an additional way to optimize the querying of additional record information, along with having to make an additional query.

The following is an example of a custom Selector method for the ContestantsSelector class, where Contestant records are queried along with the related Driver record fields. The resulting Contestant__c objects expose the Driver__r field, which provides an instance of Driver__c populated with the Driver__c fields specified in DriversSelector:

```
public List<Contestant__c> selectByIdWithDriver(Set<Id> driverIds) {
    fflib_QueryFactory contestantFactory = newQueryFactory();
    new DriversSelector().
    configureQueryFactoryFields(
    contestantFactory,
    Contestant__c.Driver__c.getDescribe().getRelationshipName());
    return Database.query(
    contestantFactory.setCondition('Id in :driverIds').toSOQL());
}
```

> The fflib_SObjectSelector.configureQueryFactoryFields method adds the selector fields to the query factory passed as the first parameter, utilizing the relationship prefix specified in the second parameter.

Due to the reuse of DriversSelector in the ContestantsSelector classes method in the last code, the following examples return consistently populated Driver__c records. This is important to the logic contained within the Domain class's Drivers.verifyCompliance method. This flexibility allows for the most optimum way of querying Driver__c records and safer reuse of the Domain class method regardless of how the records were queried.

This first example constructs the Drivers domain class with results from the standard selectById method on the DriversSelector class:

```
List<Driver__c> drivers = new DriversSelector().selectById(driverIds);
List<ComplianceService.VerifyResult> results = new Drivers(drivers).verifyCompliance();
```

This second example achieves the same, but queries the Drivers records via the custom Selector method, selectByIdWithDriver, on the ContestantsSelector class:

```
List<Driver__c> drivers = new List<Driver__c>();
List<Contestant__c> contestants = new ContestantsSelector().selectByIdWithDriver(contIds);
for(Contestant__c contestant : contestants)
    drivers.add(contestant.Driver__r);
List<ComplianceService.VerifyResult> results = new Drivers(drivers).verifyCompliance();
```

Again, both of these examples have shown that even when using related fields in SOQL, the resulting records can be kept in alignment with the desired fields as expressed by the selectors. The selectByIdWithDriver method generates the following SOQL:

```
SELECT Qualification1LapTime__c, ChampionshipPoints__c, Driver__c,
	GridPosition__c, Qualification3LapTime__c, RacePosition__c,
	Name, DNF__c, RaceTime__c, Id, DriverRace__c,
	Qualification2LapTime__c, Race__c,
	Driver__r.FIASuperLicense__c, Driver__r.Name,
	Driver__r.Id, Driver__r.Team__c
FROM Contestant__c
WHERE Id in :driverIds
ORDER BY Name
```
