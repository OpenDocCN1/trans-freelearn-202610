```
component.set("v.calendar", calendar);

// Fire the RaceSelected event
varcompEvent = $A.get("e.c:RaceSelected");
compEvent.setParams({"raceId" :selectedRaceId } );
compEvent.setParams({"raceName" :selectedRaceName } );
compEvent.fire();
}
```

+   The Apex Controller method (not shown) to load the race calendar splits and correctly orders the races into two lists (based on whether or not the race has been completed already) which are then sent to the component. This reduces the amount of JavaScript needed in the controller and helper files.

# # RaceResults component

The following shows the topmost portion of the c:RaceResults component:

| DRIVER | TEAM | GRID | RACE TIME | POINTS |
|---|---|---|---|---|
| Nico Rosberg | Mercedes | 1 | 1:17:28.89 | 25 |
| Lewis Hamilton | Mercedes | 2 | 0:0:15.100 | 18 |
| Sebastian Vettel | Ferrari | 3 | 0:0:21.0 | 15 |
| Kimi Raikkonen | Ferrari | 4 | 0:0:27.600 | 12 |
| Daniel Ricciardo | Red Bull | 5 | 0:0:45.300 | 10 |
| Valtteri Bottas | Williams | 6 | 0:0:51.0 | 8 |
| Max Verstappen | Red Bull | 7 | 0:0:54.200 | 6 |
| Sergio Perez | Force India | 8 | 0:1:5.0 | 4 |
| Felipe Massa | Williams | 9 | 0:1:5.600 | 2 |
| Nico Hulkenberg | Force India | 10 | 0:1:18.700 | 1 |