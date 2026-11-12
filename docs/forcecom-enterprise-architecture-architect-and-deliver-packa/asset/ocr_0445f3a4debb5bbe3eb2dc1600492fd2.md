# # Naming conventions

By now, you're starting to get the idea about naming conventions. The Selector classes and methods borrow guidelines from other layers with a few tweaks. Consider the following naming conventions when writing the Selector code:

+   - Class names: in naming a Selector class, it typically follows the same convention as a Domain class, taking the plural name of the object it is associated with and appending the word Selector at the end; though you can group common cross object queries into a single module scoped class:
+     - Some bad examples are RaceSQLHelper and SQLHelper
+     - Some good examples are RacesSelector, DriversSelector and RacingAnalyticsSelector

+   - Method names: the select prefix is a good way to express the shared purpose of the Selector methods, followed by a description of the primary criteria and/or relationships used. As with the Service and Domain methods, avoid repeating terms already used in the class name:
+     - Some bad examples are getRecords, getDrivers, loadDrivers, selectDriversById, and selectRacesAndContestants.
+     - Some good examples are selectById, selectByTeam, selectByIdWithContestants, and selectByIdWithContestantsAndDrivers.

+   - Method signatures: Selector methods typically return a Map, List, or QueryLocator exposing the resulting SObjects, thus supporting the bulkification needs of the platform and other layers such as the Domain class constructors and Batch Apex. Method parameters reflect the parameterized aspects of the WHERE clause; again, these should also be bulkified where applicable to do so:
+     - Some bad examples are:
+       ```
+       selectById(Id recordId)
+       DriverSelector.select(Set<Id>teamIds)
+       Database.QueryLocatorqueryForBatch(Set<Id> ids)
+       ```
+     - Some good examples are:
+       ```
+       selectById(Set<Id>raceIds)
+       selectByTeam(Set<Id>teamIds)
+       ```