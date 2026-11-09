# Action Mappings

You will use the Action Mappings in the majority of cases — this is the easiest way to configure physical ports since the Consume Adapter Service Wizard does most of the job. For each operation that you selected at the design time, the Consume Adapter Service Wizard will generate a corresponding Operation entry inside the BtsActionMapping wrapper and place the mapping into the physical port binding file it will generate. In the general form, the action mapping looks as follows:

```
<BtsActionMapping>
  <Operation Name ="Operation1" Action = "Action1" />
  <Operation Name = "Operation2" Action = "Action2"/>
  ...
  <Operation Name = "OpertionN" Action = "ActionN"/>
</BtsActionMapping>
```

To process the actions mappings and attach the SOAP actions tags to incoming messages, the WCF-Custom adapter uses the following algorithm:

When the outbound message leaves an orchestration, the orchestration instance sets the BTS.Operation context property with the logical port operation name. Once the subscription mechanism pulls the message from the message box and hands it over to the physical port for processing, the WCF-Custom adapter tries to look up the actions map for the Operation with the Name matching the BTS.Operation property. If a match is found, then the adapter attaches the corresponding Action value to the message and sends it to the custom binding for execution. If a match was not found, then the WCF-Custom adapter treats the entire BtsActionMapping as one big action, so the custom binding will get the following action:
`<BtsActionMapping><Operation Name="Operation 1" Action =......</ BtsActionMapping>` with quite a predictable consequence – "Invalid action" exception. So, the rule is clear: for each orchestration outbound operation, there must be an Operation entry in the corresponding physical port action mappings with the Name attribute value matching the orchestration operation name.