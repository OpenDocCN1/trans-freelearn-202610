# Other Considerations

# Context Properties

Much like other adapters, the Windows SharePoint Services Adapter context properties are also available inside Orchestrations. This becomes extremely useful when building dynamic send port scenarios.

![](bbox=[0.322, 0.279, 0.677, 0.506])

# Windows SharePoint Services Adapter is a one-way adapter

When configuring Receive Locations or Send Ports that use the Windows SharePoint Services Adapter, we can do so using One Way Receive Locations or One Way Send Ports. What is interesting about this scenario is that we know that the Adapter leverages two way web services. This is not saying that BizTalk will be unaware of any failures that occur when SharePoint is not available or there are other runtime issues like security failures. These errors continue to get caught and reported by BizTalk. However, if you look in the Tracked service instances, within the BizTalk Administration console, you will only find one record for each connection to SharePoint.