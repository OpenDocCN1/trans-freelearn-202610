# mcastBindAddress

This defines the IP address of the network interface to which the multicasting has to be bound to. Multicasting would be done using this interface. This parameter will only be taken into consideration when the `membershipScheme` is set to `multicast`. Note that this can be different from the `localMemberHost` parameter. So we can listen for point-to-point messages on the `localMemberHost` network interface, while listening for multicast messages on the network interface bound to the `mcastBindAddress`.

# localMemberHost

This defines the hostname or IP address of this member. This is the IP address advertised by this member when it joins the group and sends messages. This should be set to a valid value other than localhost or 127.0.0.1. In most cases, it would suffice to set it to the IP address bound to the network interface, which is used for communicating with members in the group.

# localMemberPort

This defines the TCP port used by this member. This is the port through which other members will contact this member. The default value for this port is 4000.

# preserveMessageOrder

This parameter indicates that message ordering should be preserved. This will be done according to the sender order. The default value for this parameter is true.

# atmostOnceMessageSemantics

This parameter indicates that atmost once message processing semantics need to be maintained. It guarantees that a given clustering message will be delivered at most once to the target process or node. The default value for this parameter is true.