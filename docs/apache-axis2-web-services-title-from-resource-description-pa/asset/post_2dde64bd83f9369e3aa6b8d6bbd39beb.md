
## synchronizeAll

When a web service request is received, and processed, should we update the states of all members in the cluster prior to the response being sent to the client? If the value of this parameter is set to true, the response to the client will be sent only after all the members have been updated. Obviously, this can be time consuming. In some cases, where this overhead may not be acceptable, the value of this parameter should be set to false. The risk in this case will be that all members will not be in the same state when the response is sent to the client. This condition will have to be handled by the user. In summary, this parameter defines whether or not we should synchronize the state of all members in the group before we send the response to the client.

## maxRetries

This defines the maximum number of times we need to retry to send a message to a particular member before giving up and considering that node to be faulty. After reaching the maxRetries number of retries, we give up.

## mcastAddress

This is the multicast address to be used. This parameter will only be taken into consideration when the membershipScheme is set to multicast.

## mcastPort

This is the multicast port to be used. This parameter will only be taken into consideration when the membershipScheme is set to multicast.

## mcastFrequency

This defines the frequency of sending membership multicast messages. The value should be specified in milliseconds. This parameter will only be taken into consideration when the membershipScheme is set to multicast.

## memberDropTime

This defines the time interval within which, if a member does not respond, the member is deemed to have left the group. The value should be specified in milliseconds.

## mcastBindAddress

This defines the IP address of the network interface to which the multicasting has to be bound to. Multicasting would be done using this interface. This parameter will only be taken into consideration when the `membershipScheme` is set to `multicast`. Note that this can be different from the `localMemberHost` parameter. So we can listen for point-to-point messages on the `localMemberHost` network interface, while listening for multicast messages on the network interface bound to the `mcastBindAddress`.

## localMemberHost

This defines the hostname or IP address of this member. This is the IP address advertised by this member when it joins the group and sends messages. This should be set to a valid value other than localhost or 127.0.0.1. In most cases, it would suffice to set it to the IP address bound to the network interface, which is used for communicating with members in the group.

## localMemberPort

This defines the TCP port used by this member. This is the port through which other members will contact this member. The default value for this port is 4000.

## preserveMessageOrder

This parameter indicates that message ordering should be preserved. This will be done according to the sender order. The default value for this parameter is true.

## atmostOnceMessageSemantics

This parameter indicates that atmost once message processing semantics need to be maintained. It guarantees that a given clustering message will be delivered at most once to the target process or node. The default value for this parameter is true.

## properties

There are properties specific to this member. These properties are simply name-value pairs. When a member joins groups, these properties are bound to this member so that other members in the group can detect these properties. These properties can be used in cases where central cluster management needs to be carried out.

For example: Let's think of a situation where an Axis2 cluster can be managed by a separate server. Each Axis2 member will provide two member properties, namely, backendServerURL and mgtConsoleURL. The backendServerURL property is used to connect to the backend management web services of a member. Similarly, if this member wishes to expose its own management console, it can do so by providing the "mgtConsoleURL" property. The backendServerURL property is specified using the <property name="backendServerURL" value="https://${hostName}:${httpsPort}/services/"/> entry. Here, ${hostName} signifies the hostName member property and ${httpsPort} denotes the httpsPort member property. hostName and httpsPort are two implicit member properties. httpPort is another implicit member property. Properties that have been previously declared can be used in subsequent property definitions. For example, we may define a new property as follows: <property name="foo" value="${backendServerURL}/foo"/>

## State management

The stateManager element needs to be enabled if you are required to synchronize state across members in a cluster group. An implementation of the org.apache.axis2.clustering.state.StateManager needs to be provided as the value of the class attribute. In the default Apache Tribes-based implementation, we provide the org.apache.axis2.clustering.state.DefaultStateManager class. The user can choose not to replicate certain properties. This is done by providing a property name pattern. Let us look at the following example:

```
<stateManager class="org.apache.axis2.clustering.state.DefaultStateManager" enable="true">
  <replication>
    <defaults>
      <exclude name="local_*"/>
      <exclude name="LOCAL_*"/>
    </defaults>
    <context class="org.apache.axis2.context.ConfigurationContext">
      <exclude name="UseAsyncOperations"/>
      <exclude name="SequencePropertyBeanMap"/>
    </context>
    <context class="org.apache.axis2.context.ServiceGroupContext">
      <exclude name="my.sandesha.*"/>
    </context>
  </replication>
</stateManager>
```

![](img/e2fb05de168053e6227dab034eff9353_233_0.png)

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

![](img/e2fb05de168053e6227dab034eff9353_233_1.png)

![](img/e2fb05de168053e6227dab034eff9353_235_0.png)

The cluster manager needs to have the following configuration in order to manage the two groups, Group H and Group D, as shown in the previous figure:

```
<groupManagement enable="true">
    <applicationDomain name="GroupH"
        description="This is the first group" agent="org.apache.
axis2.clustering.management.DefaultGroupManagementAgent"/>

    <applicationDomain name="GroupD"
        description="This is the second group" agent="org.apache.
axis2.clustering.management.DefaultGroupManagementAgent"/>
</groupManagement>
```

## Highly available load balancing

In order to avoid single points of failure, the worker Axis2 clusters — **Group H** and **Group D** — can be fronted by a load balancer cluster, **Group LB**, as shown in the following figure. One load balancer in **Group LB** will be the primary load balancer. The Apache Synapse load balancer can be deployed in this manner, and uses the underlying Axis2 clustering mechanism. The web service clients will talk to the endpoints in the primary load balancer and will be unaware of the backend worker node Axis2 clusters.

![](img/e2fb05de168053e6227dab034eff9353_236_0.png)

## The Axis2 clustering management API

Axis2 clustering is designed in a way that all functionality is abstracted out by a set of interfaces. By implementing these interfaces and providing the implementation classes in the clustering configuration section, any different clustering implementation can be plugged in. The default clustering implementation in Axis2 is based on Apache Tribes (Apache Tribes n.d.), the popular group management framework used by Apache Tomcat (Apache Tomcat n.d.). One may be required to use a group management framework of one's choice, in which case, he/she can implement these interfaces and then plug them using the clustering configuration section in the `axis2.xml` file. We will be taking a look at the usages of these interfaces later in this chapter.
