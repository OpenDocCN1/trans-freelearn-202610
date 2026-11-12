


"name": "Spain",
"contestants": [
{
"driverId": "44",
"championshipPoints": 44,
"dnf": false,
"qualification1LapTime": 123,
"qualification2LapTime": 124,
"qualification3LapTime": 125
}]}
}]
}
}

## 不使用工作单元

本示例展示了一种使用 `Map` 和 `List` 导入数据的传统方法，遵循批量化处理并按依赖顺序插入数据。以下代码中高亮显示的关键逻辑将导入数据结构中的数据复制到对象中；其余部分纯粹用于管理事务范围和插入依赖关系：

```
public static void importSeasons(String jsonData) {
Savepoint serviceSavePoint = Database.setSavePoint();
try{
// Parse JSON data
SeasonsData seasonsData =
(SeasonsData) JSON.deserializeStrict(jsonData,
SeasonService.SeasonsData.class);
// Insert Drivers
Map<String, Driver__c> driversById =
new Map<String, Driver__c>();
for (DriverData driverData : seasonsData.drivers)
driversById.put(driverData.driverId, new Driver__c(
Name = driverData.name,
DriverId__c = driverData.driverId,
Nationality__c = driverData.nationality,
TwitterHandle__c = driverData.twitterHandle));
insert driversById.values();
// Insert Seasons
Map<String, Season__c> seasonsByYear =
new Map<String, Season__c>();
for(SeasonData seasonData : seasonsData.seasons)
seasonsByYear.put(seasonData.year,
new Season__c(
Name = seasonData.year, Year__c = seasonData.year));
insert seasonsByYear.values();

// Insert Races
Map<String, Race__c> racesByYearAndRound =
new Map<String, Race__c>();
for(SeasonData seasonData : seasonsData.seasons)
for(RaceData raceData : seasonData.races)
racesByYearAndRound.put(seasonData.Year + raceData.round,
new Race__c(
Season__c = seasonsByYear.get(seasonData.year).Id,
Name = raceData.name));
insert racesByYearAndRound.values();

// Insert Contestants
List<Contestant__c> contestants =
new List<Contestant__c>();
for(SeasonData seasonData : seasonsData.seasons)
for(RaceData raceData : seasonData.races)
for(ContestantData contestantData
: raceData.contestants)
contestants.add(
new Contestant__c(
Race__c = racesByYearAndRound.get(
seasonData.Year + raceData.round).Id,
Driver__c = driversById.get(
contestantData.driverId).Id,
ChampionshipPoints__c =
contestantData.championshipPoints,
DNF__c = contestantData.dnf,
Qualification1LapTime__c =
contestantData.qualification1LapTime,
Qualification2LapTime__c =
contestantData.qualification2LapTime,
Qualification3LapTime__c =
contestantData.qualification3LapTime
));
insert contestants;
} catch (Exception e) {
// Rollback any data written before the exception
Database.rollback(serviceSavePoint);
// Pass the exception on
throw e;
}
```

> 如前章所述，此数据导入需求本可以通过使用 Salesforce Data Loaders 和外部字段引用来实现。您选择这种方法的一个原因是，可以从您自己的 UI 提供一个更易用的选项，供没有管理员权限的普通用户使用。

## 使用工作单元

在深入了解 FinancialForce Apex Enterprise Pattern 库如何帮助我们实现此模式之前，让我们先看看前面所示代码的修订示例。此代码使用了一个名为 Application 的新 Apex 类。该类公开一个静态属性和方法，用于创建工作单元的实例。

> 本章的源代码包括 FinancialForce Apex Enterprise Pattern 库以及其依赖的 FinancialForce Apex Mocks 库。Apex Mocks 库将在后续章节中成为重点，届时我们将专注于为本书介绍的每种模式编写单元测试。

fflib_ISObjectUnitOfWork Apex 接口用于公开工作单元模式的功能，以便在创建记录时捕获数据库工作，因此其余逻辑更加聚焦，避免了先前示例中对 Map 和 List 以及对导入数据进行重复迭代。它还为调用方在内部批量化了工作（DML）。最后，commitWork 方法调用会在应用事务管理的同时，按正确的依赖顺序执行实际的 DML 工作。

请参考以下代码：

```
// Construct a Unit Of Work to capture the following working
fflib_ISObjectUnitOfWork uow = Application.UnitOfWork.newInstance();
// Create Driver__c records
Map<String, Driver__c> driversById =
new Map<String, Driver__c>();
for(DriverData driverData : seasonsData.drivers) {
    Driver__c driver = new Driver__c(
        Name = driverData.name,
        DriverId__c = driverData.driverId,
        Nationality__c = driverData.nationality,
        TwitterHandle__c = driverData.twitterHandle);
    uow.registerNew(driver);
    driversById.put(driver.DriverId__c, driver);
}

for(SeasonData seasonData : seasonsData.seasons) {
    // Create Season__c record
    Season__c season = new Season__c(
        Name = seasonData.year,
        Year__c = seasonData.year);
    uow.registerNew(season);

    for(RaceData raceData : seasonData.races) {
        // Create Race__c record
        Race__c race = new Race__c(Name = raceData.name);
        uow.registerNew(race, Race__c.Season__c, season);
        for(ContestantData contestantData : raceData.contestants) {
            // Create Contestant__c record
            Contestant__c contestant = new Contestant__c(
                ChampionshipPoints__c = contestantData.championshipPoints,
                DNF__c = contestantData.dnf,
                Qualification1LapTime__c = contestantData.qualification1LapTime,
                Qualification2LapTime__c = contestantData.qualification2LapTime,
                Qualification3LapTime__c = contestantData.qualification3LapTime);
            uow.registerNew(contestant, Contestant__c.Race__c, race);
            uow.registerRelationship(contestant,
                Contestant__c.Driver__c,
                driversById.get(contestantData.driverId));
        }
    }
}

// Insert records registered with uow above
uow.commitWork();
```

以下是 Application 类和 UnitOfWork 静态属性的实现。它利用一个简单的工厂类，通过 newInstance 方法动态创建 fflib_SObjectUnitOfWork 类的实例：

```
public class Application
{
    // Configure and create the UnitOfWorkFactory for
    // this Application
    public static final fflib_Application.UnitOfWorkFactory
        UnitOfWork = new fflib_Application.UnitOfWorkFactory(
        new List<SObjectType> {
            Driver__c.SObjectType,
            Season__c.SObjectType,
            Race__c.SObjectType,
            Contestant__c.SObjectType
        });
}
```

fflib_Application.UnitOfWorkFactory 类公开了 newInstance 方法，该方法在内部创建一个新的 fflib_SObjectUnitOfWork 实例。这不会直接暴露给调用方，而是返回 fflib_ISObjectUnitOfWork 接口（这种设计有助于我们将在后续章节中讨论的模拟支持）。此接口的目的是提供前面代码中用于注册记录以执行插入、更新或删除的方法，并实现 commitWork 方法。

我建议您创建一个像之前所示的单一应用程序 Apex 类，在其中维护应用程序使用的所有对象及其依赖顺序的完整列表；随着应用程序对象数量的增长，维护将变得更加容易。

fflib_ISObjectUnitOfWork 接口包含以下方法。前面的示例使用 registerNew 和 registerRelationship 方法来插入记录，并确保填充适当的关系字段。有关更多详细信息，请查看代码中这些方法的文档。以下屏幕截图显示了方法摘要：

```
fflib_ISObjectUnitOfWork
^ registerNew(SObject) : void
^ registerNew(List<SObject>) : void
^ registerNew(SObject, SObjectField, SObject) : void
^ registerRelationship(SObject, SObjectField, SObject) : void
^ registerDirty(SObject) : void
^ registerDirty(List<SObject>) : void
^ registerDeleted(SObject) : void
^ registerDeleted(List<SObject>) : void
^ commitWork() : void
^ registerWork(IDoWork) : void
^ registerEmail(Email) : void
```

调用 `registerDirty` 方法并传入要更新的 `SObject` 记录，调用 `registerDeleted` 方法来删除给定的 `SObject` 记录。您也可以组合调用 `registerNew`、`registerDirty` 和 `registerDeleted` 方法。

## 工作单元范围

为了充分利用工作单元协调数据库更新和事务管理的能力，请在服务方法的范围内维护一个工作单元实例，如上一节所示。

如果您需要调用其他也执行数据库工作的类，请务必将相同的工作单元实例传递给它们，而不是允许它们创建自己的实例。我们将在本章后面稍作探讨。

> 维护工作单元的静态实例可能很诱人，这样您可以轻松引用它而无需到处传递。这种方法的缺点是，范围会比服务层执行范围更广，因为 Apex 可以在执行上下文中多次调用。使用静态执行范围工作单元注册记录的代码，依赖外部 Apex 代码单元来提交工作。它还会造成一种错误印象，即在执行提交后工作单元仍可用。
