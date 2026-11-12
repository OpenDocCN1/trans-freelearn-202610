You will also need to apply the @AuraEnabled method to accessors for Apex types referenced by the Controller methods:

```
public class ProvisionalResult {
    @AuraEnabled
    public Integer racePosition {get; set;}
    @AuraEnabled
    public Integer contestantId {get; set;}
    @AuraEnabled
    public String contestantName {get; set;}
}
```

In both these cases, however, there is no try/catch block, as error handling is done by the client code to render the messages. When exceptions are thrown, the platform takes care of catching the exception and passing it back to the calling JavaScript code for it to display accordingly. In short, there is no need to invent your own error message handling system; just let the platform handle pass it back to the client JavaScript code for you. Remember though that you still need to handle the error message in your client side JavaScript logic; this will not be done by Visualforce for you.

# Considerations for client-side logic and Service layer logic

With the addition of more complex logic in the client via JavaScript, more attention needs to be given to Separation of Concerns. The following points provide some guidelines for this:

+   Single service method per remoting method: A single Service layer method invocation rule should still be observed when developing remoting methods on the controller. If multiple Service layer methods are being called, this becomes a sign that a new aggregate Service layer method should be created and called instead.

+   Avoid remoting client bias: Do not be tempted to design your Service layer solely around your JavaScript Remoting controller method needs (the needs of your JavaScript client). Always consider the Service layer as client-agnostic; this becomes particularly important if you also want to also later expose your Service layer as an API to your application.