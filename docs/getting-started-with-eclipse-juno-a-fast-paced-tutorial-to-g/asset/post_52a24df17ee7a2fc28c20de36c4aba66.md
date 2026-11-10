
## Eclipse Development

The plugins tab allows you to select the plugins that will be loaded in the runtime workbench, both from your Workspace and from your development environment. This can be useful to test your workspace projects individually without having to close them, or to remove the unrelated plugins from the development environment to save your machine's resources. The following screenshot displays the Plugins tab of the **Run Configurations** menu:

![](img/d7c3f2018603810fae83e1a6fd98749e_186_0.png)

In the **Configuration** tab, you can set the `config.ini` file that you want to use for the runtime workbench instance, as well as modify the configuration area. The **Tracing** tab allows you to enable tracing for each plugin. Tracing is an interesting feature that allows you to turn on or off debug messages during runtime. This can be particularly useful if you are providing support for an Eclipse-based application installed in the client's environment. You can simply require the client to enable tracing for your plugin instead of sending him or her a debug version of the plugin with debug messages enabled. We won't cover tracing in this book. The **Environment** and **Common** tabs are just the same as in a regular Java application.

## Extension points

Extension points are the mechanism that Eclipse uses to allow the plugins to extend and modify other plugins' behavior. The main goal of the extension point concept is to provide a loosely-coupled architecture, exposing a few interfaces that can be implemented by other plugins. Both the extension points and their implementations are declared in the plugin.xml file.

The Eclipse platform provides a number of these extension points to allow plugins to contribute to various elements of the IDE. We will take a look at some of these extension points throughout this chapter, but a complete list of them can be found in the eclipse documentation, which can be found at http://help.eclipse.org/indigo/topic/org.eclipse.platform.doc.isv/reference/extension-points/index.html.

Our Hello World plugin implements one of these extension points. Let's see how it looks like by opening the Extensions tab in the plugin manifest editor.

![](img/d7c3f2018603810fae83e1a6fd98749e_187_0.png)

The Extensions tab shows us that, in order to add a button to the toolbar or a new entry to a menu, our plugin implements an extension point called org.eclipse.ui.actionSets. You can have more information about this extension point by selecting it in the extensions list and clicking on Show extension point description.

### Declaring an extension point

Currently, our HelloWorldPlugin project only implements extension points provided by other plugins, but doesn't declare any extension points itself. Let's learn how to do it by expanding our Hello World plugin to declare one. We will create an extension point that allows other plugins to provide the message displayed when clicking on the toolbar button.

To create a new extension point, select the **Extension Points** tab in the plugin manifest editor and click on the **Add** button in it. You will be prompted to provide three properties: **ID**, **Name**, and **Schema**. The ID is an internal unique identifier of your extension point. To guarantee its uniqueness, you might want to use the reverse domain name notation to create the ID, containing the domains from less to more specific to which your extension point belongs. Let's use `com.packt.messageProvider` as ID. As you fill this property, the **Schema** is automatically filled with the same content, followed by the `.xsd` extension. The **Point Name** property is a human-readable name for the extension point. Let's use `Message provider` here. When you're done, click on **Finish**. A new editor called schema editor will be opened. The following screenshot shows an example of schema editor:

![](img/d7c3f2018603810fae83e1a6fd98749e_188_0.png)

Schemas are XML files that describe extension points. These files are identified by a `.xsd` extension and, just like we did with `plugin.xml`, `build.properties`, and `MANIFEST.MF` files, we will use an editor to add content to it instead of editing it directly. The schema editor contains three horizontal tabs: **Overview**, **Definition**, and **Source**.

The first horizontal tab contains a basic description of your plugin. It also allows you to add documentation to your extension point, which will be shown directly in the Eclipse interface, making it easy for other developers to find information about how to implement your extension point. Remember the `org.eclipse.ui.actionSets` description that we saw in the **Extensions** tab? That's where it comes from. It's very important to thoroughly describe your extension point, as well as provide usage examples and API information.

Now, we have to describe what the extension point contains, that is, what other plugins can provide to our plugin. For this, we have to add new elements to the extension point. Open the **Definition** tab of the schema editor to see a list of the extension point's elements.

Click on **New Element** and change the new element's name to `hello_message`. You can add a small description to the element. Now add an attribute to this element by selecting it and clicking on **New Attribute**. Rename it to `value`. The other properties of the attribute allow you to choose the type that must be provided (string, Boolean, resource, Java classes, or identifiers). In our case, we want the extensions to provide a string. Your **Definition** tab should look as follows:

![](img/d7c3f2018603810fae83e1a6fd98749e_189_0.png)

Once the element is defined, we must declare how it composes the extension point. If we had another type of message (a goodbye_message, for example), we could require that the extension point implementation had one or more of each. Element compositions are achieved by adding choices and sequences to the "extension" element. Choices are an "or" composition, while sequences are an "and" composition. These elements can be used together to create more sophisticated compositions.

Since we just want one `hello_message` element per extension point implementation, let's add a new sequence to the extensions element by selecting it and clicking on **New Sequence**. Configure both **Min Occurrences** and **Max Occurrences** to be 1. Now, right-click on the **Sequence** entry, and select new | `hello_message` to declare that we need a `hello_message` element implementation.

Choices work in a very similar fashion. Add a choice to extension element and add elements to this choice. If any of the elements inside the choice are satisfied, the extension point is successfully implemented.

### Providing interfaces

One of the most utilized mechanisms of extension points is to provide interfaces that can be implemented by other plugins. To give an example of how this works, let's modify our extension point a little: instead of requiring a string from other plugins, we'll ask for a class that implements a IMessageProvider interface.

Let's start by creating the interface in our plugin. The interface must be placed inside a package that will be exported in order to be implemented by other plugins, so make sure you create a new package to store the interface. Create a new interface in the project's src folder called IMessageProvider.java with the following content:

```
public interface IMessageProvider {

    public String getHelloMessage();

}
```

As you can see, the interface only requires a getHelloMessage() method that returns a string to be implemented.

Now let's modify the extension point's schema to require an interface implementation instead of a string. To begin with, the name "value" doesn't make much sense anymore, so let's change it to message_provider. Now change the Type property from string to Java. Two new properties will appear: Extends and Implements. Since we are providing an interface, let's fill the Implements field (remember that classes are extended and interfaces are implemented). Click on the Browse button and select our IMessageProvider. Remember that you can use the filter to easily find it. Click on OK.

The following screenshot shows how our extension point will look in the end:

![](img/d7c3f2018603810fae83e1a6fd98749e_190_0.png)
