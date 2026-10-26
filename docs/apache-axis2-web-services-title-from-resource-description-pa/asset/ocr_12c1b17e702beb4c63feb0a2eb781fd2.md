# Invoking the service (second time):

```
serviceClient.sendReceiveNonBlocking(createElement("echo2"),callback2);
```

# Sending the last message:

```
clientOptions.setProperty(SandeshaClientConstants.LAST_MESSAGE,
"true");
Callback callback3 = new TestCallback ("Callback3");
```

# Invoking the service (third time):

```
serviceClient.sendReceiveNonBlocking(createElement("echo3"),callback3);
while (!callback3.isComplete()) {
    Thread.sleep(1000);
}
Thread.sleep(2000);
}
```

# Creating the payload :

```
private static OMElement createElement(String text) {
OMFactory fac = OMAbstractFactory.getOMFactory();
OMNamespace applicationNamespace = fac.createOMNamespace(tns,"ns1");
OMElement echoStringElement = fac.createOMElement(echoString,
applicationNamespace);
OMElement textElem = fac.createOMElement("in",applicationNamespace);

textElem.setText(text);
echoStringElement.addChild(textElem);

return echoStringElement;
}
```

# Implementing the Callback class:

```
static class TestCallback extends Callback {
String name = null;
public TestCallback (String name) {
this.name = name;
}

public void onComplete(AsyncResult result) {
SOAPBody body = result.getResponseEnvelope().getBody();
```