

## 处理器与阶段规则

正如我们讨论过的，模块是处理器的集合，因此一个模块可以包含一个或多个处理器。无论模块中有多少个处理器，`module.xml` 都提供了一种便捷的方式来指定它们。最重要的是，`module.xml` 可用于提供足够的配置选项，将处理器添加到系统中，并指定模块作者希望处理器在何处运行。在第4章“执行链”中，我们了解了阶段规则（phase rules）作为一种机制，可以让 Axis2 将处理器放入执行链中的特定位置，因此现在是时候通过一个示例来深入了解它们了。

在编写阶段规则以及在 `module.xml` 中指定处理器之前，让我们先看看如何编写处理器。在 Axis2 中有两种编写处理器的方式：

- 实现 `org.apache.axis2.engine.Handler` 接口
- 继承 `org.apache.axis2.handlers.AbstractHandler` 抽象类

在本章中，我们将编写一个简单的应用程序，以便更好地理解模块。此外，为了让示例应用程序更简单，我们将忽略 Handler API 的一些复杂性。我们将采用继承 `AbstractHandler` 的方式。当我们继承这个抽象类时，只需要实现一个名为 `invoke` 的方法即可。下面的示例代码将演示如何实现 `invoke` 方法：

```
public class IncomingCounterHandler extends AbstractHandler implements CounterConstants {
public InvocationResponse invoke(MessageContext messageContext) throws AxisFault {
    //get the counter property from the configuration context
    ConfigurationContext configurationContext = messageContext.getConfigurationContext();
    Integer count = (Integer) configurationContext.getProperty(INCOMING_MESSAGE_COUNT_KEY);
    //increment the counter
    count = Integer.valueOf(count.intValue() + 1);
    //set the new count back to the configuration context
    configurationContext.setProperty(INCOMING_MESSAGE_COUNT_KEY, count);
    //print it out
    System.out.println("The incoming message count is now " + count);
    return InvocationResponse.CONTINUE;
}
}
```

```
AxisModule axisModule) throws AxisFault {
    //initialize our counters
    System.out.println("inside the init : module");
    initCounter(configurationContext, INCOMING_MESSAGE_COUNT_KEY);
    initCounter(configurationContext, OUTGOING_MESSAGE_COUNT_KEY);
}

private void initCounter(ConfigurationContext configurationContext,
                         String key) {
    Integer count = (Integer) configurationContext.getProperty(key);
    if (count == null) {
        configurationContext.setProperty(key, Integer.valueOf("0"));
    }
}

public void engageNotify(AxisDescription axisDescription) throws AxisFault {
    System.out.println("inside the engageNotify " + axisDescription);
}

public boolean canSupportAssertion(Assertion assertion) {
    //returns whether policy assertions can be supported
    return false;
}

public void applyPolicy(Policy policy,
                         AxisDescription axisDescription) throws AxisFault {
    // Configuure using the passed in policy!
}

public void shutdown(ConfigurationContext configurationContext) throws AxisFault {
    //do cleanup - in this case we'll write the values of the counters to a file
    try {
        SimpleDateFormat format = new SimpleDateFormat(TIMESTAMP_FORMAT);
        File countFile = new File(COUNT_FILE_NAME_PREFIX + format.format(new Date()) + FILE_SUFFIX);
        if (!countFile.exists()) {
            countFile.createNewFile();
        }
```

```java
Properties props = new Properties();
props.setProperty(INCOMING_MESSAGE_COUNT_KEY,
                  configurationContext.getProperty(INCOMING_MESSAGE_COUNT_KEY).toString());
props.setProperty(OUTGOING_MESSAGE_COUNT_KEY,
                  configurationContext.getProperty(OUTGOING_MESSAGE_COUNT_KEY).toString());
//write to a file
props.store(new FileOutputStream(countFile), COUNTS_COMMENT);
} catch (IOException e) {
    //if we have exceptions we'll just print a message and let
it go
    System.out.println("Saving counts failed! Error is " +
e.getMessage());
}
}
}
}
```

如我们所见，前面的模块实现类中有许多方法。但值得注意的是，并非所有方法都在模块接口中。模块接口只有以下方法，而这里我们还有一些其他方法用于支持与计数器模块相关的内容：

- `init`
- `engageNotify`
- `applyPolicy`
- `shutdown`

在系统启动时，`init` 方法会被调用，此时模块可以执行各种初始化任务。在我们的示例模块中，我们初始化了入向计数器和出向计数器。

当我们将这个特定模块应用到整个系统、某个服务或某个操作时，`engageNotify` 方法会被调用。此时，模块可以决定是否允许这种应用。举例来说，如果我们尝试将安全模块应用到某个服务，而此时模块发现加密算法存在冲突，那么在这种情况下，模块将无法接入，模块会抛出异常，Axis2 也不会接入该模块。在我们的示例中，`engageNotify` 方法内部不做任何操作。

你可能已经知道，WS-Policy 是关键标准之一，在 Web 服务配置中扮演着重要角色。当你将某个模块应用到某个服务时，模块策略应当应用于该服务，并且在查看该服务的 WSDL 时应可见。因此，`applyPolicy` 方法会在我们应用模块时，将模块策略设置到相应的服务或操作上。在这个特定示例中，我们没有任何与模块关联的策略，因此同样无需担心这个方法。

正如我们在 `init` 方法中讨论的，当系统需要关闭时，`shutdown` 方法会被调用。因此，如果我们希望在该时刻执行任何类型的处理，可以将相关逻辑添加到那个特定方法中。在我们的示例中，为了演示目的，我们添加了将计数器值存储到文件中的代码。

## 编写 module.xml 文件

到目前为止，我们已经编写了两个处理器和模块实现类。现在，唯一剩下的事情就是编写模块描述文件。在编写 `module.xml` 时，我们必须使用阶段规则来指定处理器的位置，之前我们已经讨论过阶段规则（是时候复习一下阶段规则了）。我们模块的最简单的 `module.xml` 文件如下所示：

```
<module name="counterModule" class="org.apache.axis2.sample.module.request.CounterModule">
    <Description>
        Counts the incoming and outgoing messages
    </Description>
    <InFlow>
        <handler name="IncomingMessageCountHandler" class="org.apache.axis2.sample.module.request.IncomingCounterHandler">
            <order phase="Transport" after="RequestURIBasedDispatcher" before="SOAPActionBasedDispatcher"/>
        </handler>
    </InFlow>
    <OutFlow>
        <handler name="OutgoingMessageCountHandler" class="org.apache.axis2.sample.module.request.OutgoingCounterHandler">
            <order phase="MessageOut"/>
        </handler>
    </OutFlow>
</module>
```

在 `module.xml` 文件中，我们将模块的描述指定为“Counts the incoming and outgoing messages”（统计流入和流出的消息）；同时，它还通过阶段规则指定了我们之前实现的两个处理器。

如你所见，我们尝试将入站消息计数器放入传输阶段，具体位置是在 `RequestURIBasedDispatcher` 之后、`SOAPActionBasedDispatcher` 之前。如果你查看默认的 `axis2.xml` 文件，就会在 `inFlow` 中看到这两个处理器。同时，在出站消息中，计数器被添加到消息输出阶段，这里没有指定确切位置。

如果你仔细查看根元素，会发现一个名为“class”的属性，它指定了模块接口类。我们需要记住，这个属性是可选的，某些模块可能有也可能没有这个属性。

> 在某些情况下，模块作者需要为模块使用新的阶段。在这种情况下，可以编辑 `axis2.xml` 并指定所需的新阶段。一旦指定，就可以在 `module.xml` 中使用这些阶段。

在 `module.xml` 文件中，我们还没有讨论如何添加错误处理器。众所周知，错误处理有两个流：`InFaultFlow` 和 `OutFaultFlow`。当收到入站错误时，执行 `InFaultFlow`；同样，当有出站消息时，执行 `OutFaultFlow`。如果你想向这些阶段中的任何一个添加处理器，可以像操作另外两个流一样进行。

## 部署并应用模块

现在我们已经编写了有效模块所需的一切，剩下的唯一事情就是创建模块归档文件并将其部署到存储库中。首先编译源代码，然后，如我们所知，会生成 `.class` 文件。

假设 `org.apache.axis2.sample.module.request` 是我们源文件的包名，我们可以在 `classes/org/apache/axis2/sample/module/request` 下找到所有 `.class` 文件。

