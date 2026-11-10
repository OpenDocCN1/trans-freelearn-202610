You can create the class by simply clicking on the message_provider* link in the Extension Elements Details pane. Now copy the following code into it:

```
import helloworldplugin.IMessageProvider;

public class MyMessageProvider implements IMessageProvider {

    @Override
    public String getHelloMessage() {
        return "Hello world from MessageProvider";
    }

}
```

As you can see, this is a very basic implementation. We are now ready to test our extension point and its implementation. Select one of the two projects that we've just created and select Run | Run as | Eclipse Application. The message coming from the extension point is displayed when we click on the button, as shown in the following screenshot:

![](bbox=[0.2, 0.44, 0.8, 0.61])

# Contributing to the platform's menus and toolbars

In the example that we used to illustrate how to declare and implement an extension point, we created a plugin that adds a button to the Eclipse toolbar. Now, let's take a more careful look at how this button was added there, as well as find out how we could add elements to other parts of the Eclipse interface.