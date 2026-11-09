To illustrate the concept by a concrete example, let's refer to the source code provided with the Echo adapter. The point of our interest is the Execute method of the Microsoft.ServiceModel.Channels.Common.IOutboundHandler interface. This interface is one of the core interfaces that all ASDK-based adapters supporting outbound operations must implement. The Execute is called by the ASDK runtime to perform an outbound operation. Here is the implementation:

```
```csharp
/// <summary>
/// Executes the request message on the target system and returns a response message.
/// If there isn't a response, this method should return null
/// </summary>
/// <param name="message">The Message to be executed</param>
/// <param name="timeout">The timeout for the operation</param>
/// <returns>the response message, null if there was no response</returns>
public Message Execute(Message message, TimeSpan timeout)
{
    OperationMetadata om =
        this.MetadataLookup.GetOperationDefinitionFromInputMessageAction(message.Head
    if (om == null)
    {
        throw new AdapterException("Invalid operation " + message.Headers.Action);
    }

    switch (message.Headers.Action)
    {
        case "Echo/EchoString":
            return CreateEchoStringResponseMessage(message);

        default: throw new AdapterException("Invalid Operation " + message.Headers.Ac
    }
}
```
```

The code is self-explanatory and as you can clearly see, is action-centric. If the action coming with the SOAP header is recognized, the adapter performs the requested operation, if not, then the adapter responds with "Invalid Operation" exception. Since the Echo adapter exposes only one operation, using actions may seem unnecessary, but imagine the situation when an adapter exposes multiple operations? In such cases, using the SOAP actions is quite a logical way to help the adapter to decide what operation to perform.

There are two different ways to associate a BizTalk message with the appropriate SOAP action (or to attach the appropriate processing tag as you wish). One is to configure physical send ports using so called Action Mappings. The other is to set the actions dynamically at runtime.