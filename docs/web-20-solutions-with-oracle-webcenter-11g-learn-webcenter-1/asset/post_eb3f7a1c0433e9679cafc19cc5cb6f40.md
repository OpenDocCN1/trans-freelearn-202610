

## Discussions, Wiki, and Blog Services

Click on the **Resource Palette** and open **My Catalogs**. You should now see the **WebCenter Services Catalog**. Expand the catalog to view the various task flows available.

![](img/fccf6c511126cf79e98510edd3b343ac_180_0.png)

In the task flows shown, you see the various discussions-related task flows such as the **Discussion Forums**, **Discussions - Popular Topics**, **Discussions - Recent Topics**, and so on. We will use these task flows in our application to create a view of the Discussion Forum.

![](img/fccf6c511126cf79e98510edd3b343ac_180_1.png)

## Embedding a Discussions task flow in the JSF page

From the Resource Palette, drag the Discussions Forums task flow and drop it on to the JSF page. When prompted, select Region as the way to create the task flow.

![](img/fccf6c511126cf79e98510edd3b343ac_181_0.png)

In the Task Flow Binding dialog, enter the values for the categoryId and forumId. You can find the IDs using the Jive Forums Admin Console.

![](img/fccf6c511126cf79e98510edd3b343ac_181_1.png)

| Parameters   | Description    |
|--------------|----------------|
| categoryId   | The category ID of an existing category in your WebCenter Discussions instance to which the view should be scoped.    |
| forumId      | The identifier for an existing forum within the specified category. You should make sure that the specific forum exists. |
| showRecursiveForums | Determines if only the forums in the specified category will be shown or all the subcategories as well. If set to false (the default value) then only the forums under the given category will be shown, otherwise all the forums under the category and the subcategories will be shown.    |
| isCategoryView | If this is set to true then the task flow will list the forums grouped under the category. When it is set to false (the default), the topics associated with the forum will be shown. |

See the following screenshot to locate the CATEGORYID:

![](img/fccf6c511126cf79e98510edd3b343ac_182_0.png)

See the following image to locate the FORUMID:

![](img/fccf6c511126cf79e98510edd3b343ac_182_1.png)

If prompted to add a library, click **Add Library**.

![](img/fccf6c511126cf79e98510edd3b343ac_182_2.png)

Next, we embed the **Discussions – Sidebar View** task flow to the page using the same procedure as for **Discussions Forums**.

## Deploying, running, and testing

Now you can run the page by right clicking on main.jspx file in the Application Navigator and choosing **Run** from the context menu. The application will be deployed to the Integrated WebLogic Server and the default browser will be launched with the appropriate URL.

![](img/fccf6c511126cf79e98510edd3b343ac_183_0.png)

## Integrating Wiki and Blog with WebCenter

There are three primary methods of integrating the Oracle WebCenter Wiki and Blog Server into custom WebCenter applications. They are as follows:

1. Using an IFrame, which you include in a page in your custom WebCenter application.
2. Using any portlet capable of consuming a URL such as the Web Clipping portlet.
3. Using the Web Services provided, which requires a custom-built user interface. We will not cover this method in detail.

In this chapter, we will illustrate the use of the first two methods. For details on how to use the Web Services interface to the Wiki and Blog Server, refer to the Oracle WebCenter Developer's guide.

## Prerequisites

Before we start developing the custom WebCenter application to integrate with the Wiki and Blog Server, we should ensure that the WebCenter Wiki and Blog Server is running. If the Wiki and Blog Server is not running then start the WLS_Services managed server as described in the WebCenter Installation and Configuration chapter. Using a browser, open the URL for Wiki main page (for example, http://hostname:8890/owc_wiki).

![](img/fccf6c511126cf79e98510edd3b343ac_184_0.png)

## Integrating Wiki and Blog using an IFrame

We will see how to integrate it as follows.

## Creating a new WebCenter Application

Using JDeveloper, create a new application selecting the WebCenter Application template. The steps to be followed are similar to the ones for HelloWorldApp in Chapter 4, Getting Started with WebCenter Development.

![](img/fccf6c511126cf79e98510edd3b343ac_185_0.png)

The defaults should be fine for this application, so click **Finish** to continue.

## Creating a JSF page

Next, we will create a JSF page to host the view for the Wiki and Blog Server. In the Application Navigator, select the ViewController project, right-click on it, and from the context menu, select **New** to launch the New Gallery.

![](img/fccf6c511126cf79e98510edd3b343ac_185_1.png)

## Discussions, Wiki, and Blog Services

In the New Gallery, select JSF Page to create a new JSF page.

![](img/fccf6c511126cf79e98510edd3b343ac_186_0.png)

In the Create JSF Page dialog, select a name for the page and create the page.

![](img/fccf6c511126cf79e98510edd3b343ac_186_1.png)

## Creating an IFrame to include the Wiki and Blog Server page

In order to integrate the Wiki and Blog Server pages into your custom WebCenter application, drag the **Inline Frame** component from the **Component Palette** onto the JSF page created.

![](img/fccf6c511126cf79e98510edd3b343ac_187_0.png)

Next modify **Source** property of the IFrame to point to the Wiki and Blog Server page that you want to embed in the application. The format of the URL is http://hostname:port/owc_wiki/page/show.jz?page=domain:wikiPageName. Here we have used the StartPage of the domain webcentech.

![](img/fccf6c511126cf79e98510edd3b343ac_188_0.png)

We are now ready to view the page. Right-click on the main.jspx file in the Application Navigator and click **Run** from the context menu. The Web Browser is launched and shows the login page for the Wiki and Blog Server. Use any valid username and password to log in to the Wiki and Blog Server.

![](img/fccf6c511126cf79e98510edd3b343ac_188_1.png)

## Integrating Wiki and Blog using the Web Clipping Portlet

In the following sections, we will demonstrate how to add the Wiki and Blog Server to a custom WebCenter application using the Web Clipping Portlet. Oracle WebCenter Framework includes a Web Clipping Portlet that we can register with our custom WebCenter application. On running the application in the browser we can consume the Wiki and Blog Server URL using the Web Clipping Studio.

## Adding a new JSF page to the application

Add a new JSF page to the application using the procedure described previously.

![](img/fccf6c511126cf79e98510edd3b343ac_189_0.png)

## Registering the Web Clipping Portlet

From Application Resources, select Connections | New Connection | Oracle PDK-Java Producer.

![](img/fccf6c511126cf79e98510edd3b343ac_189_1.png)

Next specify the connection details. The default URL for the Web Clipping Portlet is http://hostname:port/portalTools/webClipping/providers/webClipping. The defaults for the other configuration parameters are acceptable, so click **Finish** to complete the registration.

![](img/fccf6c511126cf79e98510edd3b343ac_190_0.png)

Once the Web Clipping Portlet is registered, you can see its entry under Connections.

![](img/fccf6c511126cf79e98510edd3b343ac_190_1.png)

### Adding the Web Clipping Portlet to the JSF page

Drag the Web Clipping Portlet from Connections to the portlet.jspx page. This will add the portlet to the page.

![](img/fccf6c511126cf79e98510edd3b343ac_191_0.png)

## Running the portlet page

Right-click on portlet.jspx page in Application Navigator and from the context menu select Run to run the page. The browser will be launched showing the Web Clipping Studio. Next, we need to customize the Web Clipping Studio and input the Wiki and Blog Server URL. Click on the dropdown at the top-right corner of the page to open the Web Clipping Portlet menu.

![](img/fccf6c511126cf79e98510edd3b343ac_191_1.png)

From the menu, select Customize. This will open the form to enter the URL location.

![](img/fccf6c511126cf79e98510edd3b343ac_191_2.png)

