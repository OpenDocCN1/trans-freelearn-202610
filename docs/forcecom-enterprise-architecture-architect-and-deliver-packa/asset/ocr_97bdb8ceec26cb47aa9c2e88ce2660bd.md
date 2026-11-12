+   - Class names: Use the plural name of your Custom Object for the name of your Domain class. This sets the tone for the scope of the record that the class deals with as clearly being bulkified, as described in the following subsection.
+     - Some bad examples are Race.cls and RaceDomain.cls
+     - Some good examples are Races.cls and Teams.cls

+   - Method names: Methods that relate to the logic associated with database operations or events should follow the onEventName convention. Other method names should be descriptive and avoid repeating the name of the class as part of the name.
+     - Some bad examples are Races.checkInserts and Races.startRace
+     - Some good examples are Races.onBeforeInsert and Races.start

+   - Parameter Names and Types: Much of what was described in the previous chapter applies here. Though passing in a list of records to the Domain class methods is not required, this is available as a class member variable to all the domain class methods, as you will see later in this chapter.
+     - Some bad examples are:
+       ```
+       Races.onBeforeInsert(
+           List<Race__c>racesBeingInserted)
+       Races.start(
+           List<Id>raceIds)
+       ```
+     - Some good examples are:
+       ```
+       Races.onBeforeInsert()
+       Races.start()
+       ```

+   - Inner classes and interfaces: Much of what was described in the previous chapter also applies here. As the data being managed is expressed in terms of the actual Custom Object themselves and that information is available as a class member variable, there is typically less of a need for inner classes representing data to be passed in and out of a Domain class.