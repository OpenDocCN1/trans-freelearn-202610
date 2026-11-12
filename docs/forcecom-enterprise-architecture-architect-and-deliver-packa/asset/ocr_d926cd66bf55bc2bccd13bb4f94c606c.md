By using method overloading, a new version of the existing ContestantService.awardChampionshipPoints method was added to implement the preceding code without impacting other callers. The overloaded method takes as its first parameter the Unit Of Work (created in RaceService) and then the Contestant list.

> The problem with this additional method is that it inadvertently exposes an implementation detail of the service to potential non-service calling code. It would be ideal if Apex supported the concept of the protected keyword found in other languages, which would hide this method from code outside of the Service layer. The solution is to simply ensure that developers understand that such method overrides are only for use by other Service layer callers.

The following resultant code shows that the ContestantService methods now support both the new RaceService caller and the original ContestantController caller. Also, there is only one instance of Unit Of Work in either code path, and the commitWork method is called once all the work is done:

```
public static void awardChampionshipPoints(Set<Id> contestantIds)
{
    fflib_ISObjectUnitOfWork uow =
    Application.UnitOfWork.newInstance();

    // Apply championship points to selected contestants
    awardChampionshipPoints(uow,
    new ContestantsSelector().selectById(contestantIds));

    uow.commitWork();
}
```

```
public static void awardChampionshipPoints(
fflib_ISObjectUnitOfWork uow, List<Contestant__c> contestants)
{
    // Apply championship points to given contestants
    Map<Integer, ChampionshipPoint__mdt> pointsByTrackPosition =
    new ChampionshipPointsSelector().selectAllByTrackPosition();
    for(Contestant__c contestant : contestants) {
    // Determine points to award for the given position
    ChampionshipPoint__mdt pointsForPosition =
    pointsByTrackPosition.get(
    Integer.valueOf(contestant.RacePosition__c));
    if(pointsForPosition!=null) {
```