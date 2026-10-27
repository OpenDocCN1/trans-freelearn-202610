# Other debugging techniques

Let's take a look at some other debugging techniques that may be very useful:

+   - When your web script does not work, you should also check whether it is properly registered to the Alfresco web script engine or not. You can hit `http://localhost:8080/alfresco/service/index` and locate your web script from the various options available to browse web scripts, such as browse all by package or by URI. When you click on it, it will display the descriptor, JavaScript controller, and response template. You can make sure that the JavaScript controller has all your fixes or modifications that you added to fix the issue.
+   - In your web script, when you are working on some properties of a node, permissions on a node, and so on, you can also get help from `nodebrowser`. Using `noderef`, you can find out the details like properties set on the node, its children, aspects applied on it, associations with other nodes, and permissions on a node that might also be useful to you while debugging for your issue fix. You can also use other functionalities available in `nodebrowser` such as the ability to execute lucene queries that might also be helpful when you want to verify the lucene query used by the web script controller.

# Troubleshooting pointers

Having understood the different ways to debug web scripts in Alfresco, let's take a look at some of the troubleshooting pointers for some common issues you might come across while working with Alfresco web scripts.

# A valid SecureContext error

You might have sometimes faced the error **A valid SecureContext was not provided in the RequestContext**. There can be many reasons for getting this error. Some of them are listed as follows:

+   - When you have set authentication to `none` in the web script description document, and in the controller you are trying to invoke some public services of Alfresco that require authentication, for example, calling the `toDisplayPath()` method, it will give you the mentioned exception.
+   - If you have a JavaScript-backed controller for your web script and you have deployed it using the repository option and authentication is set to `none`, you will get the mentioned exception when you try to hit your web script.