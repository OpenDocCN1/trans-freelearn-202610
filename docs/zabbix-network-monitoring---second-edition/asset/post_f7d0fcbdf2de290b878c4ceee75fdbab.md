
Let's say we have two servers, A test host and Another host, providing a redundant SSH File Transfer Protocol (SFTP) service. We would be interested in any one of the services going down. Navigate to Configuration | Hosts, and click on Triggers next to either A test host or Another host. Then, click on the Create trigger button. Enter these values:

- **Name:** One SSH service is down.
- **Expression:** Click on the Add button. In the resulting popup, click on Select next to the Item field. Make sure Another host is selected in the Host dropdown, click on the SSH server status item in the NAME column, and then click on Insert. Now, position the cursor at the end of the inserted expression and enter ` or ` without quotes (that's a space, or, and a space). Again, click on the Add button. In the resulting popup, click on Select next to the Item field. Select A test host from the Host dropdown, click on the SSH server status item in the NAME column, and click on Insert.
- **Severity:** Average (remember, these are redundant services).

The final trigger expression should look like this:

```text
{Another host:net.tcp.service[ssh].last()}=0 or {A test host:net.tcp.service[ssh].last()}=0
```

When you are done, click on the Add button at the bottom.

> In Zabbix versions preceding 2.4, a pipe character, `|`, was used instead of a lowercase `or`.

The process we followed here allowed us to create a more complex expression than simply comparing the value of a single item. Instead, two values are compared, and the trigger fires if either of them matches the comparison. That's what the `or` operator does. Another logical operator is `and`. Using the SSH server as an example trigger, we could create another trigger that would fire whenever both SSH instances go down. Getting the expression is simple, as we just have to modify that single operator, that is, change `or` to `and`, so that the expression looks like this:

```text
{Another host:net.tcp.service[ssh].last()}=0 and {A test host:net.tcp.service[ssh].last()}=0
```

> Trigger expression operators are case sensitive, so `AND` would not be a valid operator—a lowercase `and` should be used.

Trigger expressions also support other operators. In all the triggers we created, we used the most common one: the equality operator, `=`. We could also be using the inequality operator, `<>`. That would allow us to reverse the expression, like this:

```text
{A test host:net.tcp.service[ssh].last()}<>1
```

> Zabbix versions preceding 2.4 used the hash mark `#` instead of `<>` for the “not equal” comparison.

While not useful in this case, such a trigger is helpful when the item can have many values and we want the trigger to fire whenever the value isn't the expected one.

Trigger expressions also support the standard mathematical operators `+`, `-`, `*`, and `/`, and comparison operators `<`, `>`, `<=`, and `>=`, so complex calculations and comparisons can be used between item data and constants.

Let's create another trigger using a different function. In the frontend section Configuration | Hosts, choose Linux servers from the Group dropdown, click on Triggers next to A test host, and click on the Create trigger button. Then, enter these values:

- **Name:** `Critical error from SNMP trap`
- **Expression:** `{A test host:snmptrap.fallback.str(Critical Error)}=1`
- **Severity:** High

When you are done, click on the Add button at the bottom.

This time, we used another trigger function, `str()`. It searches for the specified string in the item data and returns 1 if it's found. The match is case sensitive.

This trigger will change to the OK state whenever the last value for the item does not contain the string specified as the parameter. If we want to force this trigger to the OK state manually, we can just send a trap that does not contain the string the trigger is looking for. Sending a success value manually can also be useful when some other system is sending SNMP traps. In a case where the problem trap is received successfully but the resolving trap is lost (because of network connectivity issues, or for any other reason), you might want to use such a fake trap to make the trigger in question go back to the OK state. If using the built-in trap-processing functionality, it would be enough to add trap information to the temporary file. If using the scripted solution with Zabbix trapper items, `zabbix_sender` could be used. SNMP trap management was discussed in Chapter 4, Monitoring SNMP Devices.

## Preventing trigger flapping

With the service items and triggers we wrote, the triggers would fire right away as soon as the service is detected to be down. This can be undesirable if we know that some service will be down for a moment during an upgrade because of log rotation or backup requirements. We can use a different function to achieve a delayed reaction in such cases. Replacing the function `last()` with `max()` allows us to specify a parameter and thus react only when the item values have indicated a problem for some time. For the trigger to fire only when a service has not responded for 5 minutes, we could use an expression such as this:

```text
{A test host:net.tcp.service[ssh].max(300)}=0
```

> For this example to work properly, the item interval must not exceed 5 minutes. If the item interval exceeds the trigger function's checking time, only a single value will be checked, making the use of a trigger function such as `max()` useless.

Remember that for functions that accept seconds as a parameter, we can also use the count of returned values by prefixing the number with `#`, like this:

```text
{A test host:net.tcp.service[ssh].max(#5)}=0
```

In this case, the trigger would always check the five last-returned values. Such an approach allows the trigger period to scale along in case the item interval is changed, but it should not be used for items that can stop sending in data.

Using trigger functions is the easiest and most-applied solution to potential trigger flapping. The previous service example checked that the maximum value over the last 5 minutes was 0; thus, we were sure that there are no values of 1, which would mean “service is up”.

For our CPU load trigger, we used the `avg(180)` function, checking the average value for the last 3 minutes. We could also have used `min(180)`—in this case, a single drop below the threshold would reset the 3-minute timer even if the overall average were above the threshold. Which one to use? That is entirely up to you, depending on what the functional requirements are. One way is not always better than the others.

## Checking for missing data

Some items are always expected to provide values, such as the CPU load item. The problem condition for this item usually is “value too large”. But there are some items that are different, for example, an item with the `agent.ping` key. This item only tells us whether the agent is available to the server, and it only returns 1 when the agent is up. And yes, that's it—it does not send 0 when the agent is down; there is no value at all. We can't write a trigger with the `last()` function, as the last value is always 1. The same goes for `min()`, `max()`, and `avg()`. Luckily, there is a function we can use in this case: `nodata()`. It allows the trigger to fire if an item is missing data for some period of time. For example, if we created an `agent.ping` item on “A test host”, the trigger could look like this:

```text
{A test host:agent.ping.nodata(300)}=1
```

Here, the `nodata()` function is checking whether this item is missing data for 300 seconds, or 5 minutes. If so, the trigger will fire. What's the comparison with 1? All trigger functions in Zabbix return some number. The `nodata()` function returns 1 if the item is missing data and 0 if there's at least one value in the specified time period. Note that it might not be a good idea to try and guess what return values are available for some trigger function—if you are not sure, better check the manual for details at https://www.zabbix.com/documentation/3.0/manual/appendix/triggers/functions.

The `nodata()` function is said to be time based. “Normal” trigger functions are evaluated when an item receives a new value. This makes a lot of sense for triggers against items such as the CPU load item we created earlier—when a value arrives, we compare it to the threshold. It wouldn't work that well with our `agent.ping` item, though. If values were coming in, everything would be good—the trigger expression would be evaluated, and this function would return 0. If values stopped coming in, it would not get evaluated and would never fire. Then, if a new value arrived, the function would get evaluated and would see that new value and declare that everything was perfect.

So in this case, the trigger is not evaluated only when a new value comes in. Instead, this function is evaluated every 30 seconds. This interval is hardcoded. To be more specific, any trigger that includes at least one time-based function in the expression is recalculated every 30 seconds. With the 30-second interval, one should never use a parameter lower than 30 for the `nodata()` function. To be safe, never use a parameter lower than 60 seconds. In Zabbix version 3.0.0, the following trigger functions are time-based:

- `date()`
- `dayofmonth()`
