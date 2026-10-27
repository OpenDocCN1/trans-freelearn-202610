# Code examples

Now that you have information on the different root objects you can use in the JavaScript controller, let's take a look at some code examples to get a better understanding of how you can put the root objects in action to achieve the required functionality. We will go through some code examples for some of the generic functionalities that every developer will use at some point in time. We will use some of the root objects we have gone through previously.

# Creating a folder

Consider a scenario where you have a custom frontend application and Alfresco as the backend repository. Now, you want to have the functionality for the users to create a folder in Alfresco under the company home from the custom frontend. Once the folder is created, you might also want to create some content inside the folder or run some action on the folder. First, our goal is to create a folder under the company home.

To address this, we will create a new web script foldercreation, which will accept the argument foldername and creates a named folder under the company home and returns the noderef value as a response in the XML format. You could use the returned noderef further to invoke other web scripts as required, for example, to run an action or create content.

In subsequent sections, you will find the core code snippet that needs to be implemented inside the JavaScript controller and response template in order to achieve the mentioned scenario. You can also go through the code files provided for this chapter in order to see the web script implementation for this web script.

# Retrieving explicit arguments

In order to implement this scenario, as the first thing in the JavaScript controller, we need to read the argument foldername. Explicit arguments can be retrieved using the args root object. So, we will read the argument foldername as follows:

```
if (args.foldername != null && args.foldername != "")
```

> If you have implicit arguments provided in the web script URL, then these arguments will not be available using the args root object. For them, you can get their values using the url root object like in url.templateArgs.foldername.