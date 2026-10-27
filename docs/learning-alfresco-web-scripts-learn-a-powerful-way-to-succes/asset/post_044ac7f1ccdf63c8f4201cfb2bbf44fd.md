
It's now time to practically implement your first web script in Alfresco and get more familiar with Alfresco web script implementation. This chapter will help you get a clear understanding on how to create a simple web script in Alfresco, how to extend web script with controller implementation, and some fundamental things about web script implementation that are very important to know while working on Alfresco web scripts.

Consider a scenario where you are managing the project execution of developing an integration solution of a custom frontend application with Alfresco as backend repository. Now, you have been allocated a new developer to develop Alfresco web scripts who is not very familiar with web script implementation and you want to get him up to speed for web script development. Never mind, you can have him read through this chapter and he should be able to get a good understanding of developing web scripts in Alfresco after reading this chapter.

## Creating your first web script in Alfresco

Whenever we start learning about any new programming language, the first program we implement is the "Hello world" program wherein we simply print the text **Hello world** as the output of the program. We will also do the same thing here. We will create a basic web script in Alfresco that will simply display **Hello world!** as the output of the web script.

In order to create a basic and simple web script in Alfresco, you will need to have a descriptor for the web script and a FreeMarker template to render the output of the web script.

Web scripts can be created from the available Alfresco user interfaces or you can have them developed outside of Alfresco and then deploy them. As we are going to implement a very basic web script here, we will use the Alfresco user interface option and will create a web script. You can either use Alfresco Explorer web client or Alfresco Share UI for this. However, as Alfresco Explorer is now not being encouraged to be used in general as it is not on the roadmap of Alfresco as no new implementations are being made in support of Alfresco Explorer, we will also not use Alfresco Explorer. Instead, we will use the Alfresco Share UI which is recommended to use in general.

Let's now create the web script. In order to create your first web script in Alfresco, follow these steps:

- 1. Log in to Alfresco Share UI.
- 2. Click on the **Repository** link from the top header.
- 3. Go to **Data Dictionary** | **Web Scripts Extensions**.
- 4. Create a new file named helloworld.get.desc.xml with content as follows:

```
<webscript>
  <shortname>Hello World</shortname>
  <description>First webscript Hello world</description>
  <url>/helloworld</url>
</webscript>
```

Downloading the example code
You can download the example code files for all Packt books you have purchased from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

- 5. Create a new file named helloworld.get.html.ftl with content as follows:

```
<html>
  <body>
    <p>Hello world!</p>
  </body>
</html>
```

- 6. Now, hit the URL http://localhost:8080/alfresco/service/index in a browser. Provide the username and password to the authentication pop up displayed. Click on the Refresh Web Scripts button available at the bottom.
- 7. Click on the List Web Scripts link. You can see the Hello world web script by clicking on the Browse all Web Scripts link and search for helloworld.
- 8. Now, hit the URL http://localhost:8080/alfresco/service/helloworld. Here we go, you should see the output Hello world! rendered in your browser.

We have implemented the first web script in Alfresco. Now, let's try to understand what we have just done in order to implement the first web script. We first created the web script as follows:

- - We implemented a GET web script to display Hello World! as the output
- - We created the descriptor document helloworld.get.desc.xml for the web script
- - We also created the FreeMarker template helloworld.get.html.ftl to render the web script response

In general, in order to create a web script in Alfresco, a minimum two files are required. One is the descriptor document and the other is FreeMarker template.

## It's Time for the First Web Script

Next, we deployed the web script as follows:

- - We have deployed the required files to create a web script in the Alfresco repository
- - The descriptor document and FreeMarker template are placed in **Data Dictionary | Web Scripts Extensions**

> Web script files can be directly created/uploaded in the Alfresco repository in **Data Dictionary | Web Scripts Extensions** or web script files can be created and deployed on the filesystem through the code base as well.

We then registered the web script as follows:

- - We hit the URL `http://localhost:8080/alfresco/service/index` and clicked on the **Refresh Web Scripts** button to register the first web script

> There are two ways to register a web script. One is through hitting the URL `http://localhost:8080/alfresco/service/index` and clicking on the **Refresh Web Scripts** button. Another way is to deploy the web script files through the code base/repository and restart the server. A server restart will also register the web scripts.

Finally, we executed our first web script as follows:

- - We invoked the web script from the web browser hitting the URL `http://localhost:8080/alfresco/service/helloworld`
- - The web script displayed an output as its response on a web browser

## Understanding the web script URI

It is important to understand the web script URI. The way the web script URI can be represented in a generic form is `http[s]://<host>:<port>/[<contextPath>/]/<servicePath>/[<scriptPath>] [?<scriptArgs>]`

The terms used in the web script URI are explained as follows:

- - `http[s]`: This is the protocol to invoke the web script. This could be either `http` or `https`.
- - `host`: This is the name or address of the server where the web script is deployed.
- - `port`: This is the port where the web script is exposed on the server hosting the web scripts.
- - `contextPath`: This is the path where the application is deployed to. For data web scripts, this would generally be `/alfresco`.
- - `servicePath`: This is the path where the web script service is mapped with. Generally, this would be `/service`.
- - `scriptPath`: This is the path to the web script as defined in the web script descriptor document `*.desc.xml` under the `<url>` tag.
- - `scriptArgs`: These are the arguments to be passed to the web script as generally defined in the web script descriptor document `*.desc.xml` under the `<url>` tag. Arguments can be specified after the web script `scriptPath` just after ?.

Now you understand the different terms that make up the web script URI, now you can easily understand the URI of our first web script by mapping our web script URI to the generic form of the web script URI. Note that we were not having arguments added to our first web script, hence, you would not be able to map the `scriptArgs` part. Let's now take a look at what needs to be done to add the arguments to a web script.

## Adding arguments to a web script

Let's say we want to modify our first web script to display the name too in the web script response, for example, **Hello! Ramesh**. It can be done in a simple way by passing the name as the URL argument of the web script and then using this argument to generate the response in the FreeMarker template. In order to do this, we will have to perform the following steps:

- 1. Log in to Alfresco Share UI.
- 2. Click on the **Repository** link from the top header.
- 3. Edit the descriptor file `helloworld.get.desc.xml` at **Data Dictionary | Web Scripts Extension** location as follows:
```
<webscript>
    <shortname>Hello World</shortname>
    <description>First webscript Hello world</description>
    <url>/helloworld?name={argumentName}</url>
</webscript>
```
- 4. Edit the FreeMarker template `helloworld.get.html.ftl` at **Data Dictionary | Web Scripts Extension** as follows:
```
<html>
    <body>
```
