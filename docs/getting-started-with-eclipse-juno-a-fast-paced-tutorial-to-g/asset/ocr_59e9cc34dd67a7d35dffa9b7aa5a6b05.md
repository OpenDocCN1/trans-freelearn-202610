# Implementing an extension point

Now that we have created the extension point, let's create a new plugin that implements it. Start by creating a new plugin project named MessageProvider. We don't want to use any templates this time, so unmark the Create a plugin using one of the templates checkbox in the Templates page of the new plugin project wizard.

Before we start working with the extension point, we have to declare that the MessageProvider plugin depends on HelloWorldPlugin, If there's no HelloWorldPlugin in the platform, there's no extension point to implement and MessageProvider plugin is pointless. To declare the dependency, let's open the plugin manifest editor for the MessageProvider project and switch to the Dependencies tab. Click on the Add button in the Required Plug-ins section and type HelloWorldPlugin. The following screenshot shows how the Dependencies tab should look like:

![](bbox=[0.18, 0.39, 0.82, 0.61])

Now we have to declare that our plugin contains an implementation of the messageProvider extension point. Switch to the Extensions tab, and click on the Add button. Select the com.packt.messageProvider extension point.

The next step is to actually implement the messageProvider extension point. As you can see, the com.packt.messageProvider extension already contains a hello_message element. The Extension Element Details pane shows us what we already know. We need a class that implements IMessageProvider to implement this extension point.