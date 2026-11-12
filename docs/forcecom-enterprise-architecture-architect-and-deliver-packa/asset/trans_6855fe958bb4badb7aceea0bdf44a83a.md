
> fflib_QueryFactory 类方法提供了构建动态 SOQL 查询时使用标准字符串拼接或 String.format 方法的替代方案。fflib_QueryFactory 类的原始作者 Chris Peterson 创建它的动力在于，避免基于字符串的方法可能带来的脆弱性，并提供一种更易读的方式来理解此类代码的作用。此外，由于该类支持使用 SObjectField 引用，而非字符串中的字段名引用，因此此类代码不易受到 SOQL 注入漏洞的影响。

## 自定义 Selector 方法的基本示例

首先，我们来看一个基本示例，该示例根据分配给 Driver__c 的团队进行查询。按照指南，参数已批量化，如果需要，允许你查询多个团队中的驱动程序。

```
public List<Driver__c> selectByTeam(Set<Id> teamIds) {
    return (List<Driver__c>)
    Database.query(
    newQueryFactory().
    setCondition('Team__c in :teamIds').
    toSOQL();
    }
```

通过使用 newQueryFactory 方法，fflib_QueryFactory 实例已预先填充了选择器指定的字段列表、对象名称和 order by 子句的知识。因此，此 Selector 方法在已填充字段和返回记录的顺序方面表现一致。

## 带有子查询的自定义 Selector 方法

在以下示例中，查询使用子查询来包含子记录（在此示例中，Race 中的 Contestants）。关键点在于，RacesSelector 方法（如以下代码所示）重用 ContestantsSelector 类的实例来访问其字段列表并执行安全检查：

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

前面的代码使用 fflib_SObjectSelector 方法 addQueryFactorySubselect。该方法创建一个新的 fflib_QueryFactory 实例，并将其传递给传入方法的查询工厂（在此示例中为 racesQueryFactory）。在外部查询工厂上调用 toSOQL 方法时，它会自动为任何包含的查询工厂生成子查询。

通过重用 ContestantsSelector 的实例，确保无论如何查询 Contestant__c 记录列表（直接查询或作为子查询的一部分），字段都能一致地填充。ContestantSelector 上的 getOrderBy 方法也被使用，以确保即使记录作为子查询的一部分返回，Contestant 记录的默认顺序也能保持不变。

让我们再看一下之前使用的示例，以说明此模式如何避免了不一致查询的风险。虽然 Contestant__c 记录来自两个不同的选择器（RacesSelector 和 ContestantsSelector），但由于前面的实现中重用了 ContestantsSelector 类，因此在已填充字段方面结果相同。

以下示例显示标准 selectById 方法被用于查询 Contestants，以便将其传递给 Contestants 域类构造函数，这是服务层实现中颁发冠军积分的一部分：

```
List<Contestant__c> contestants =
    new ContestantsSelector().selectById(contestantIds);
new Contestants(contestants).awardChampionshipPoints(uow);
```

以下替代示例显示 RaceSelector 上的自定义 selectByIdWithContestants 方法被用于查询 Races 和 Contestants，以便将其传递给 Contestants 域类构造函数以实现相同的结果：

```
List<Contestant__c> contestants = new List<Contestant__c>();
for(Race__c race :
    new RacesSelector().selectByIdWithContestants(raceIds)) {
    contestants.addAll(race.Contestants__r);
}
new Contestants(contestants).awardChampionshipPoints(uow);
```

在这两个示例中，无论记录是作为直接查询还是子查询来查询，字段和记录排序都是一致的。这让开发者更有信心使用最佳方法来查询当时所需的记录。

selectByIdWithContestants 方法生成以下 SOQL：

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

## 带有关联字段的自定义 Selector 方法

除查询子记录外，还可以使用 SOQL 字段点表示法来查询关联记录。这提供了一种额外的方式来优化查询其他记录信息，而无需进行额外查询。

以下是 ContestantsSelector 类的自定义 Selector 方法示例，其中查询 Contestant 记录及其关联 Driver 记录字段。生成的 Contestant__c 对象会公开 Driver__r 字段，该字段提供一个填充了 DriversSelector 中指定的 Driver__c 字段的 Driver__c 实例：

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

> fflib_SObjectSelector.configureQueryFactoryFields 方法将选择器字段添加到作为第一个参数传递的查询工厂中，使用第二个参数中指定的关系前缀。

由于上述代码中 ContestantsSelector 类方法重用了 DriversSelector，以下示例返回一致填充的 Driver__c 记录。这对于域类 Drivers.verifyCompliance 方法中包含的逻辑非常重要。这种灵活性允许以最佳方式查询 Driver__c 记录，并更安全地重用域类方法，而不考虑记录是如何查询的。

第一个示例使用 DriversSelector 类上的标准 selectById 方法的结果来构建 Drivers 域类：

```
List<Driver__c> drivers = new DriversSelector().selectById(driverIds);
List<ComplianceService.VerifyResult> results = new Drivers(drivers).verifyCompliance();
```

第二个示例实现相同结果，但通过 ContestantsSelector 类上的自定义 Selector 方法 selectByIdWithDriver 查询 Drivers 记录：

```
List<Driver__c> drivers = new List<Driver__c>();
List<Contestant__c> contestants = new ContestantsSelector().selectByIdWithDriver(contIds);
for(Contestant__c contestant : contestants)
    drivers.add(contestant.Driver__r);
List<ComplianceService.VerifyResult> results = new Drivers(drivers).verifyCompliance();
```

再次，这两个示例都表明，即使在 SOQL 中使用关联字段，生成的记录也可以与选择器表达的所需字段保持一致。selectByIdWithDriver 方法生成以下 SOQL：

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
