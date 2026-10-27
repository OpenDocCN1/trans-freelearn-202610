It is really important to know two things: how you can deploy what you have developed and how you can debug if something does not work the way it should. If you know these two things, then your job becomes easier and you can quickly identify the solution to some of the critical issues you are working on for your project.

While working on web scripts in Alfresco, the first phase of web script development is the creation of web scripts, wherein you will develop the required building blocks of a web script. The second most important phase is to deploy and register the web script in order to make the web script you have created available to the web script framework in Alfresco. This will enable you to make use of your web script. As a developer, it is important to understand various deployment options with which you can deploy Alfresco web scripts. Also, you should know how you can debug web scripts in Alfresco.

In this chapter, we will take a look at the possible ways you can deploy web scripts in Alfresco. Also, we will go through debugging techniques for web scripts and some useful troubleshooting pointers. We will also look at some useful tips that will be very helpful when executing web scripts for your project on a production server.

# # Deployment options

There are two possible ways you can have your web scripts deployed:

+   - Using the repository option
+   - Using the filesystem option

When creating web scripts in Alfresco, you might be creating Java-backed or JavaScript-backed web scripts for your project. Both can be deployed in different ways. It is important to know where you can deploy a web script with each deployment option and also which option is more suitable to you.

# ## The repository option

In simple words, deploying a web script using the repository option means putting the web script component files inside a specific location within the repository.

First, let's take a look at the deployment of JavaScript-based web scripts. JavaScript-based web scripts can be deployed in the Alfresco repository. All the components for the JavaScript-based web script such as the description document, JavaScript-backed controller implementation, one or more response templates, configuration documents if any, and optional message bundles can be deployed in the Alfresco repository.