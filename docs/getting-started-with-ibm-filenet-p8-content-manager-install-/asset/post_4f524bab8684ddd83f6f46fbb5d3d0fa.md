
## Installing the Content Engine

Once the installer has completed, you'll find an entry for FEM on the Windows **Start** menu and a shortcut for it on the desktop. Be sure that you have installed WebSphere 3.0, as noted above, before launching FEM. Without it, FEM will run, but it will be unable to connect to the CE.

FEM can connect to any number of CE servers in any number of Domains. You have to tell it where the CE is (the server URL) and, of course, supply login credentials. On your very first run of FEM, it won't know any of those things, and so you cannot get past the logon screen without adding at least one set of connectivity information.

![](img/2295191c450a0c86b2d82df6cec04526_106_0.png)

When you click the **Add** button, you'll see a configuration panel like the following, where you can enter connectivity details. Several fields are pre-populated. You are likely to only need to supply the **Nickname** and modify the **Server** and **Port** values for your local environment. Recall that our CE is running in a second profile in our WAS server, and all of the port numbers are shifted up by one from the default values. So, rather than take the WAS default of 9080 or 9443, change it to 9081 or 9444 for HTTP or HTTPS, respectively. Notice that you can change it either in the **Port** field or the **URL** field, and the other field is automatically adjusted.

The **Connection** field offers choices of **http** and **https**. Even if your local security policy doesn't require it, it's a good idea to get in the habit of using **https** (in other words, a connection protected by TLS/SSL). Your CM data might not be particularly sensitive, but your administrator credentials are sent over that connection (except for the case of integrated login).

![](img/2295191c450a0c86b2d82df6cec04526_107_0.png)

### Bringing up the P8 Domain

On your first FEM connection to the new CE installation, you will be guided through a series of steps that we informally call Domain bring-up. It's the final phase of configuration that leads to a functional Domain. The very first step in that is the choice of a name for the Domain. The Domain name doesn't show up in many places because the use of a given CE connection URL implies a particular Domain. So, you can call it pretty much anything you want. For our examples, we'll call our Domain Lucky.

The next step is the wizard for creating a directory configuration. Based on how much time we spent with directory data in the Configuration Manager and the WAS admin console, you could probably type this information in your sleep. The earlier steps were done so that WAS could do authentication. The CE steps are done so that the CE can resolve group memberships for authorization and also so it can service user and group searches done via CE APIs. We won't belabor the attribute values again, but the main configuration panel looks as follows:

![](img/2295191c450a0c86b2d82df6cec04526_108_0.png)

The user directory configuration properties look like this:

![](img/2295191c450a0c86b2d82df6cec04526_108_1.png)

To launch the wizard, you can either click on the **Create an Object Store** link pictured above, or you can select the **Object Stores** node, right-click, and select **New Object Store** from the context menu. An Object Store has both a display name and a symbolic name, with the symbolic name having a restricted character set. They need not be the same, and they need not have any relationship to the name of the database nor the WAS datasources, though it is customary to align those names for bookkeeping convenience. We'll use the display name Object Store One and the symbolic name OStore1.

You'll recall that we created two datasources via the Configuration Manager— FNOSTORE1DS and FNOSTORE1DSXA. Supply those values when asked. The **Advanced** button on that wizard panel leads to some specialty database settings that are not of interest here.

For the default content store, you could select **Database Storage Area**. That would be fine for a small demo system like the one that we're creating, but it's not realistic for most production sites. Most (but not all) real-world content is stored in either a **File Storage Area** or a **Fixed Storage Area** (a reference to a specialized hardware device). Select **File Storage Area**. In the panel that follows, either type or browse to `/p8filestores/fs1`. Recall that we created that directory in *Chapter 2*. (Actually, as we didn't create an actual fileshare for that directory, you won't be able to browse to it. You will have to enter the name directly. It's a directory on the CE server machine, not the client machine where FEM is running.) You can take the defaults for the next panel that gives File Storage Area parameters.

The next few panels are concerned with establishing the initial security for the Object Store. The list of Object Store administrators should already be pre-populated with the gcd_admin user. Use the **Add** button and the search box to add the object_store_admin_group group. Recall that group includes the object_store_admin user, so it's not necessary to add that user individually.

For the initial users and groups who are allowed to connect to this Object Store, we want to be fairly lax for our demo system. Use the **Add** button and the search box to add the #AUTHENTICATED-USERS group. This is not a real group from the directory. It's a pseudo-group representing any authenticated user. In other words, anyone who can successfully log in via the TDS directory can connect to this Object Store. Finer-grained security can be used to restrict access to business objects. We'll cover that in *Chapter 8* when we talk at length about security.

The final step is the selection of AddOns to install into the Object Store. An AddOn is a collection of classes and properties, typically in support of some specific application. We would ordinarily caution you to select **Minimal Configuration** because it's easy to install additional AddOns later. For our all-in-one system, it's nice to have lots of things to look at, so click **Workplace/Workplace XT**, which selects almost all of the available AddOns.

![](img/2295191c450a0c86b2d82df6cec04526_112_0.png)

When you see the upbeat message in the figure above, your Object Store will be ready for use, and you will have reached the end of the journey for installing the CE. We'd suggest another five-minute break to celebrate, but let's not get all crazy.

### Summary

In this chapter, we successfully installed the CE into an all-in-one environment. That built on top steps we did in *Chapter 2*; configuring the operating system, and installing and configuring a database system, a directory server, and a J2EE application server. Once the CE was installed and running, we installed the administrative tool, FEM, and used it to initialize our Domain and create our first Object Store.

Now that you've done all that once with most of the choices made for you, you can feel comfortable using the PPG and installation worksheet to try it with different software components or with different choices for things.

In the next chapter, we'll use FEM to explore some basic aspects of this CE system that is still bare bones at this point.

## 4 Administrative Tools and Tasks

In *Chapters 2 and 3*, we spent quite a bit of effort to set up the environment and install the **Content Engine (CE)**. In *Chapter 5*, we'll spend time installing additional components and tools to round out the **Content Manager (CM)** environment. Let's take a little break in this chapter and see some features of CM in action.

Although we will be using **FileNet Enterprise Manager (FEM)** to illustrate most of these features, remember that these are really features of the CE itself. As such, they are sometimes available in other tools and always available via the CE APIs. The point is not to show how to operate FEM itself, but to illustrate CM features. Because FEM is an administrator's tool, we'll concentrate mostly on administrative concepts and tasks.

Recall that FEM must run on a Microsoft Windows machine. Even if you are using virtual machine images or other isolated servers for your CM environment, you might wish to install FEM on a normal Windows desktop machine for your own convenience.

In this chapter, we'll cover:

- A discussion of the P8 Domain and what's in it
- How to use topology levels to configure your environment
- A discussion of the objects that reside directly within a Domain
- A discussion of an Object Store and what's in it
- An example of creating a custom class and adding custom properties to it
