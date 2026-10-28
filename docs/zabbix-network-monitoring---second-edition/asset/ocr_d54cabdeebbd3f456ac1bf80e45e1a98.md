The field already contained two macros — {TRIGGER.STATUS} and {TRIGGER.NAME}. The benefit of a macro is evident when we have a single action covering many cases. We don't have to create a myriad of actions to cover every possible situation; instead we use macros to have the desired information, related to the particular event, replaced. Macro names usually provide a good idea of what a macro does. In this case, we improved the existing subject line, which already contained trigger name and status macros, by adding the host name macro, though it is still recommended to include the host name in trigger names.

To confirm your changes, click on Update. Make the trigger change state by sending SNMP traps like before, then check your e-mail. The subject now includes the host name. But wait, now the host name is included twice — what have we done? The subject is now:

```
PROBLEM: SNMP trap has arrived on snmptraps on snmptraps
```

We used the same macro in the trigger name and in the action subject. You should decide where you would like to specify the host name and always follow that rule.

There's also something else slightly strange in the e-mails — at the end of the message body, there are some lines with UNKNOWN in them:

```
Received SNMP traps (snmptraps:snmptraps): 192.168.56.11 "Critical Error"
NET-SNMP-MIB::netSnmpExperimental
*UNKNOWN* (*UNKNOWN*:*UNKNOWN*): *UNKNOWN*
*UNKNOWN* (*UNKNOWN*:*UNKNOWN*): *UNKNOWN*
```

If we now look at the corresponding action configuration:

```
Item values:
{ITEM.NAME1} ({HOST.NAME1}:{ITEM.KEY1}): {ITEM.VALUE1}
{ITEM.NAME2} ({HOST.NAME2}:{ITEM.KEY2}): {ITEM.VALUE2}
{ITEM.NAME3} ({HOST.NAME3}:{ITEM.KEY3}): {ITEM.VALUE3}
```

The number that is appended in these macros, such as in {ITEM.NAME1}, is the sequential number of the item in the trigger expression. The trigger that sent the notifications for us referenced a single item only, thus the first reference works, referencing the second and third items fails, and that outputs *UNKNOWN* in the message. The default action is meant to be used as an example — in this case, demonstrating the ability to reference multiple items. If most of your triggers reference only a single item, it might be desirable to remove the second and the third lines. At this time, there is no way to conditionally print the item value, if it exists.