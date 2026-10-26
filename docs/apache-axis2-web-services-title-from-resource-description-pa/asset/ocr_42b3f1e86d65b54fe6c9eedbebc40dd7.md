![](bbox=[0.37, 0.14, 0.59, 0.27])

The static members are specified in the axis2.xml file of each member as follows:

```
<members>
    <member>
        <hostName>S1</hostName>
        <port>4000</port>
    </member>
    <member>
        <hostName>S2</hostName>
        <port>4000</port>
    </member>
</members>
```

As shown in the preceding figure, a new member, N, belonging to the domain Group H and having knowledge about at least one static member in the group, can join the group. This new member will first contact one of the static members and learn about the group membership. The static member will inform N about the group membership {S1, S2, D1, D2, D3} and then notify each member in the group about N joining the group, as shown in the following sequence diagram:

![](bbox=[0.22, 0.57, 0.75, 0.78])