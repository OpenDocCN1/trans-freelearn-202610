| Text | Category | Line |
|---|---|---|
| EXECUTION_STARTED | EXECUTION_STARTED | |
| TRIGGERS | CODE_UNIT_STARTED | [EXTERNAL] |
| flforces.ContestantsTrigger on Contestant trigger event BeforeUpdate for [a03b00000006wVph] | CODE_UNIT_STARTED | [EXTERNAL] |
| flforces.ContestantsTrigger on Contestant trigger event AfterUpdate for [a03b00000006wVph] | CODE_UNIT_STARTED | [EXTERNAL] |
| EXECUTION_FINISHED | EXECUTION_FINISHED | |

In this second example, imagine that some Apex code was executed to calculate the contestants' championship points once the race ends. In this case, the code was executed from an Execute Anonymous prompt (though in the application, this would be a button); again, the Apex Debug log was captured and is shown in the next screenshot.

Again, you can see the EXECUTION_STARTED and EXECUTION_FINISHED entries, along with the three CODE_UNIT_STARTED entries: one for the RaceService.awardChampionshipPoints Apex method called from the Execute Anonymous prompt and two others for the Apex Trigger code.

```
Log
Text Category Line
EXECUTION_STARTED EXECUTION_STARTED
> execute_anonymous_apex CODE_UNIT_STARTED [EXTERNAL]
< <>entry(Trigger) SYSTEM_CONSTRUCTOR_ENTRY [1]
< RaceService.RaceService() METHOD_ENTRY [1]
< flforces.RaceService.awardChampionshipPoints(Set<Id>) METHOD_ENTRY [1]
< fl_IdtoOpptyIDMap METHOD_ENTRY [54]
< SYSTEM_CONSTRUCTOR_ENTRY [9]
< <init>(<VST>Schema.SObjectType>) [9]
< Aggregations_1.select (...) SOQL_EXECUTE_BEGIN [94]
< QueryLocatorIterator (...) SYSTEM_METHOD_ENTRY [97]
< flforces.RaceService.awardChampionshipPoints.registerDML(SObjects) METHOD_ENTRY [201]
< flforces.RaceService.$constructDMLWork.registerDML(SObjects) METHOD_ENTRY [271]
< flforces.RaceService.constructDMLWork.commitWork() METHOD_ENTRY [281]
> Savepoint_set SAVEPOINT_SET [156]
< ... DML_ENTRY [174]
< ... DML_BEGIN [177]
> flforces.ContestantsTrigger on Contestant trigger event BeforeUpdate ... CODE_UNIT_STARTED [EXTERNAL]
> flforces.ContestantsTrigger on Contestant trigger event AfterUpdate ... CODE_UNIT_STARTED [EXTERNAL]
< CUMULATIVE_LIMIT_USAGE CUMULATIVE_LIMIT_USAGE
< (default) LIMIT_USAGE_FOR_NS NS
< ... LIMIT_USAGE_FOR_NS NS
< CUMULATIVE_LIMIT_USAGE_END CUMULATIVE_LIMIT_USAGE_END
< EXECUTION_FINISHED EXECUTION_FINISHED
```

What is different from the first example is, as you can see by looking at the Line column, that the execution transitions show lines of the Apex code being executed and EXTERNAL. When DML statements are executed in Apex to update the Contestants records, the Apex code execution pauses while the platform code takes over to update the records. During this process, the platform eventually then invokes Apex Triggers and once again, the execution flows back into Apex code.