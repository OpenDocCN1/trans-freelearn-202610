+   - If there is a phase rule of a handler with both `phaseFirst` and `phaseLast` set to `true`, then that particular phase cannot have more than one handler. If someone tries to write a phase rule that inserts a handler into the same phase, then the second phase rule is invalid.
+   - There cannot be two handlers in one phase with their `phaseFirst` attribute set to `true`.
+   - There cannot be two handlers in one phase with their `phaseLast` attribute set to `true`.
+   - If the rule is such that the `before` attribute is referred to as the `phaseFirst` handler, then the rule is invalid.
+   - If the rule is such that the `after` attribute is referred to as the `phaseLast` handler, then the rule is invalid.

```
<handler name="simple_HandlerError " class="org.apache.axis.
handlers.SimpleHandlerError ">
    <order phase="userphase1" before=" simple_Handler"
phaseFirst="true"/>
</handler>
```

Phase rules are defined per basic handler, and any handler in the system must fit into a phase in the system.

# Flow

Flow is simply a collection of phases, and the order of phases inside a flow is defined in `axis2.xml`. As a phase is a logical collection and, which is, in fact, a virtual concept, a flow can be assumed as the execution chain (a collection of handlers). There are four types of flows in Axis2:

+   - `InFlow`: When a message comes in (request message), the message has to go via the `InFlow`. Then all the handlers in the `InFlow` will be invoked. `InFlow` is somewhat different from the `OutFlow`. A flow consists of two parts. The first part is from the beginning to the dispatcher (up to and including the dispatch phase). The second part will be there only if a corresponding service is found at the end of the dispatch phase. Therefore, the second part of the flow is `InFlow` of the corresponding operation for the incoming message. So the `InFlow` consists of a global part and an operation part.
+   - `InFaultFlow`: This flow will be invoked if the incoming request is faulty (request with HTTP status code 500).
+   - `OutFlow`: When a message is moving out from the server (say a response), this is invoked. As a result, the outgoing message is always bound to an operation, and there is nothing similar to dispatching in the out path.