# Understanding JavaScript-based Web Scripts in Detail

The web script framework in Alfresco makes it easy to create, access, update, and delete content residing in the repository using JavaScript-backed web scripts. The JavaScript-backed web script will be very helpful when you need to do some kind of hotfixes without bringing down the production system. Another cool feature of JavaScript-backed web scripts is that you can have them deployed in the repository and can directly edit them in line using the Alfresco user interface. There is no need to depend on any specific editor while developing web scripts that have a JavaScript-backed controller. You can use any of your favorite editors to create them. The major advantage of using a JavaScript-backed web script is that it speeds up the development cycle of web scripts. You need not worry about compiling them the way you do with your Java-backed web scripts. While developing JavaScript-backed web scripts, you can leverage the Alfresco provided JavaScript APIs. There is a wide range of APIs provided by Alfresco to make repository access easy and convenient from JavaScript-controller-backed web scripts.

In this chapter, first we will find out the possible things that you can do using JavaScript APIs. We will learn how to identify the available JavaScript APIs in Alfresco. We are going to take a look at the root objects available in Alfresco. This will help us access the available JavaScript API and effectively develop web scripts in less time; also, it will make it easier to interact with the Alfresco repository. We will also look at other helpful root objects provided by the web script framework in Alfresco. We will see few code examples that will help you understand how you can make use of Alfresco JavaScript APIs in your web script. At the end, we will go through the steps to understand how to create a custom JavaScript extension, which is a very important and useful thing to know.

# Understanding what you can do with the
JavaScript API

While developing web scripts in Alfresco, the repository JavaScript APIs provided by Alfresco will help you perform a lot of the following actions at a high level as listed in the following list:

+   - Creating a folder
+   - Creating content
+   - Making a copy of a node
+   - Moving a node from one location to another
+   - Deleting a node
+   - Finding a node
+   - Performing searches against a repository