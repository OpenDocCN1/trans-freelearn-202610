
```java
mocks.startStubbing();
mocks.when(mockSelector.SObjectType()).
thenReturn(Race__c.SObjectType);
mocks.when(mockSelector.selectByIdWithContestants(
    new Set<Id> { testRaceId })).
thenReturn(testRacesAndContestants);
mocks.when(mockDomain.SObjectType()).
thenReturn(Contestant__c.SObjectType);
mocks.stopStubbing();
```

```
// Given - Inject mocks
Application.UnitOfWork.setMock(mockUow);
Application.Selector.setMock(mockSelector);
Application.Domain.setMock(mockDomain);

// When
RaceService.awardChampionshipPoints(
    new Set<Id> { testRaceId });

// Then
((RacesSelector) mocks.verify(mockSelector, 1)).
    selectByIdWithContestants(new Set<Id> { testRaceId }));
((Contestants) mocks.verify(mockDomain, 1)).
    awardChampionshipPoints(mockUow);
((fflib_SObjectUnitOfWork) mocks.verify(mockUow, 1)).
    commitWork();
}
```

Consider the following notable aspects from the preceding test code:

- The fflib_ApexMocksUtils.makeRelationship method is used to construct in memory a list of records containing child records. This utility method (part of ApexMocks) works around a platform limitation in mocking this data construct. This method is very useful for returning more complex record sets returned from selector methods your tests are mocking.

- Prior to calling the Application.Selector.setMock and Application.Domain.setMock methods to inject the mock implementations of the RacesSelector and Contestants domain classes, the mocking framework is used to mock responses to the SObjectType methods for the mocked instances of these classes, which is called by the setMock methods. Mocking this method ensures that the mocked selector instance gets correctly registered with the Selector factory.

- The fflib_IDGenerator.generate method (part of ApexMocks) is used to generate an in memory ID that is used to populate the mock records returned from the mock selector. These same IDs are used to confirm what was passed into the Selector as the same IDs were passed to the Service method.

- The Contestants domain class gained a default constructor as required by the Test.createStub method, used by the fflib_ApexMocks.factory method.

### Unit Testing a Domain method

The following test method can be found in the ContestantsTest class and demonstrates mocking selector method responses to return database rows created in memory. It also shows that asserting the same records are subsequently passed to a mocked unit of work for update:

```
@IsTest
private static void whenAwardChampionshipPointsUowRegisterDirty() {

    fflib_ApexMocks mocks = new fflib_ApexMocks();

    // Given
    fflib_SObjectUnitOfWork mockUow = (fflib_SObjectUnitOfWork)
        mocks.factory(fflib_SObjectUnitOfWork.class);
    Application.UnitOfWork.setMock(mockUow);
    Id testContestantId =
        fflib_IDGenerator.generate(Contestant__c.SObjectType);
    List<Contestant__c> testContestants =
        new List<Contestant__c> {
            new Contestant__c (
                Id = testContestantId,
                RacePosition__c = 1 ));
    // When
    Contestants contestants = new Contestants(testContestants);
    contestants.awardChampionshipPoints(mockUow);

    // Then
    ((fflib_SObjectUnitOfWork)
        mocks.verify(mockUow, 1)).registerDirty(
        fflib_Match.sObjectWith(
        new Map<SObjectField, Object>{
            Contestant__c.Id => testContestantId,
            Contestant__c.RacePosition__c => 1,
            Contestant__c.ChampionshipPoints__c => 25 }));
}
```

Consider the following notable aspects from the preceding test code:

- An in memory list of records is created in the test code and passed to the domain classes constructor. The awardChampionshipPoints method being unit tested is then called to determine if it correctly interacts with the unit of work.

- A custom ApexMocks matcher is used to check that the SObject passed to the mocked fflib_SObjectUnitOfWork.registerDirty method is the expected method. The matcher is also helping the test code confirm that the correct championship points have been calculated. For more information about ApexMocks matters refer to the earlier section.

- As this test is using a mocked unit of work, no DML is performed.

### Unit Testing a Selector Method

The main logic to be tested by selector methods is that of constructing SOQL strings dynamically. Ideally it would be good if it were possible to mock the Database.query method and thus unit tests could be written around complex selector methods to assert the SOQL strings generated. As this is currently not possible, there is little benefit in mocking selector classes. Your selector classes can instead obtain coverage and validation through their executions as part of your integration tests.

### Summary

Integration testing focuses on the scope of controller methods or APIs exposed via your services to test the full stack of your code. It requires setting up the database, executing the code to be tested and querying the database. These tests are critical to ensuring all the components of your application deliver the expected behavior.

In order to make the individual code components, classes and methods as robust and future proof as possible, developers can test each method in isolation without incurring the overhead of setting up the database or updating it. As such, unit tests run more quickly. Unit tests can increase coverage, as more corner cases testing scenarios can be emulated using mocking of scenarios that would otherwise be impossible or difficult to setup on the database.

Unit Testing requires an understanding of some other patterns such as Dependency Injection and mocking. Dependency Injection can be implemented in a number of ways, using constructors, factories and setter methods. The approach you use of Dependency Injection will depend on your personal preference and/or the preference of the libraries you're using. Multiple approaches to dependency injection can be mixed if needed.

One approach to implementing mocking is to use Apex interfaces. Utilizing multiple implementations of interfaces that represent the boundaries between the dependencies of your code base allows you to implement the real code and test code separately.

Mocking is most effective when the code being tested is not aware which implementation is being used. The Apex Stub API allows for the creation of dynamic mocks for classes without using Apex interfaces. This is good for existing code bases or scenarios where using Apex interfaces don't make sense or adds overhead.

ApexMocks is a mocking framework library that reduces the work involved in building mock objects, removing the need to physically code them. Based on Mockito it offers advanced facilities to configure mocked method responses or throw exceptions to help test error handling. This framework also records the mock methods invoked and provides a sophisticated matching syntax to assert what methods were called.

The Apex Commons open source library used in this book utilizes the factory pattern to provide built in methods for injecting mock implementations of services, unit of work, domain, and selector layers.

In the end unit testing and its associated prerequisites make it a difficult concept to master and requires a lot of work up front. Start with a few simple examples at first and continue leveraging your existing Apex testing approaches until you're ready to expand. Once you have mastered the API's and concepts, it becomes second nature and can help open up access to other development coding practices such as test-driven development.

In the final chapter of this book, we will take a step back and look at how to scale your development around multiple developers and setup continuous and automated execution of your tests whenever changes are made.

## 13 Source Control and Continuous Integration

So far we have been making changes in the packaging org for the FormulaForce application. This has worked well enough, as you're the sole developer in this case. However, when you add more developers and teams other considerations comes into play, mainly traceability of code changes and also the practical implications of conflicts when more than one developer is working in a development org at a time.

This chapter sees the packaging org take on more of a warehouse-only role, accessed only when there is a need to create a beta or release package and no longer for day-to-day development as we have been doing. This principle can become quite important to you and your customers if they desire that you demonstrate compliance and auditability with respect to controls around your software development process and life cycle.

As the code base grows and the number of contributions to it at any given time increases, it's important to monitor the quality of the code more frequently. Finding out that your Apex tests are failing prior to an upload is frustrating and costly when you're trying to release the software to your customers. Continuous Integration works with a Source Control system to validate the integrity and quality (through your tests) of your application on an automated basis, often triggered by developers committing changes to Source Control.

In this chapter, you will learn the following:

- Developer workflow
- Developing with Source Control
- Hooking up to Continuous Integration
