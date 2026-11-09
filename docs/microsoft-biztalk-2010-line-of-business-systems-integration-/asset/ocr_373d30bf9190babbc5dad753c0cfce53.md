24. For the destination schema, select the `Chapter3-DynamicsCRM.AccountIntCustomer_XML` message type. The resulting map will look like this:

![](bbox=[0.23, 0.2, 0.77, 0.5])

25. The message coming back from Dynamics CRM (which is the source of this map) contains an array of fields and values for the returned entity. The `Attributes` node contains all of the entity values. When there is an option set, or list of values, involved (e.g. `addresstypecode`) the Attributes value would be "1" while the `FormattedValues` node, which contains friendlier representations for option set values, would have an entry for `addresstypecode` where the value would be "Default Value".

26. To map the repeating `KeyValuePairOfstringanyType` to the structured destination fields, we can either build one long custom XSLT block, individual XSLT blocks for each section, or try and use other functoids (for example, **Logical** functoids with **Value Mapping**) to match the repeating source nodes to the destination node. In this example, build individual blocks of XSLT necessary to populate the entire destination schema.

27. Drag a Scripting functoid to the mapping surface and set it to use **Inline XSLT**. This functoid connects to the destination ID node and has no link from a source field. The XSLT for this functoid looks for the `value` node associated with the `key` node that holds the `contactid` value.