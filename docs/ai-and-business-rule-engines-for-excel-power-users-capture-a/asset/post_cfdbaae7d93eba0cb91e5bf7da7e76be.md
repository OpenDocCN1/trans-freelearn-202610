
Now that we’ve decided that workflow and business rules make a good end-to-end solution, let’s meet Power Automate and understand why we should choose it as our workflow tool.

## What is (and why) Power Automate?

Microsoft Power Automate (formerly known as Flow) is one of the hundreds of workflow offerings on the market – do a search for Workflow Engine if you want to look for yourself. So why does this book choose it, especially when Kogito also has a workflow engine built in?

- As a Microsoft product, Power Automate is highly integrated with Office, including Excel. The hosted version is available online, without setup, to most Office users.
- Power Automate is aimed at Excel Power users – a good crossover with the readership of this book. Meanwhile, Kogito’s workflow engine is more complex and is targeted at developers. And Power Automate strikes a nice balance – while easy to use, it is still more powerful than consumer-orientated services such as IFTTT.com.
- Power Automate follows the Business Process Model Notation (BPMN) standard, so many of the concepts you learn about in this chapter will be applicable to other graphical workflow engines. We’ll see this when we take a quick look at the Kogito workflow engine (jBPM) at the end of this chapter.

So, what is Power Automate? It’s a way of taking the workflow (as in Figure 8.1), adding the specific actions we want to carry out at each step, and then triggering (or starting) the workflow when specific conditions are true – for example, to start the workflow when we receive an email.

Because it is cloud-based, the workflows will continue to run when your laptop is turned off, even when you go on holiday. So, let’s take a look at Power Automate and create our first flow.

## Getting started with Power Automate online

Power Automate is linked from several areas of Microsoft Office, but the easiest place to start is the home page at https://powerautomate.microsoft.com/. After logging in (or creating a trial account if you need one), you’ll be greeted by a home/welcome page with a lot of sample flow templates, and pointers to learning materials on each of them.

It’s worth exploring these resources (now or later) – there are a lot of other Power Automate features outside the scope of this chapter. Instead, we’ll take a quick look around the screen before starting to build our first flow.

Click on My flows on the left-hand side of the home screen. You’ll see a screen similar to Figure 8.2, although obviously, you may not have any flows listed there (yet). This will be our main area for working with flows, so it’s worth taking a look around the screen.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_212_0.png)

Figure 8.2 – Home screen of Microsoft Power Automate

On the left-hand side is the menu we’ve already used to navigate between the Home and My flows screens. It has the option to create a flow, which we’ll use shortly, and a Templates option, which suggests pre-built solutions – that’s well worth exploring in your own time, as it shows just how much you can do with Power Automate.

We won’t cover the other options in this chapter, but we’ll explain them briefly to encourage you to look at them in your own time:

- **Approvals** are for when you’re running flows requiring a signoff, and you’re the person nominated to say yes/no.
- **Connectors** allow you to link your flow to many different systems.
- **Data** links you to the underlying Microsoft Dataverse, which will be familiar if you’ve already used Power Query or Power Apps.
- **Monitor** allows you to see when your flows ran, whether the flow completed successfully, and makes suggestions on how to resolve any problems.
- **AI Builder** links to Microsoft artificial intelligence (AI) tools – the selection here can be very specific. If Microsoft has a pre-built solution for you, then it’s well worth using it. But the options to modify them are narrow – in which case, you’re better off using other AI approaches in this book or the machine learning techniques we will touch on in Chapter 11.

There are some other menu items along the top worth exploring:

- **Cloud flows** are what we’re going to create shortly – they run online, regardless of whether your laptop is on or off.
- **Desktop flows** are an extension we’ll come to later in the chapter. They are both more powerful (anything you can do on your laptop can be automated with a desktop flow) and more limited (your laptop needs to be turned on, and they are normally triggered by a cloud flow).
- **Shared with me** allows you to collaborate and share flows with your colleagues. Please read the security warnings carefully if/when you do decide to share a flow.

Pay attention to which account you are using (top-right of the screen, not shown in the figure) – especially if you’re using a trial Power Automate account, as it can cause problems if you are using the incorrect one.

Now that we’ve taken a tour of the Power Automate screen, let’s dive in and create our first flow.

## Our first Power Automate flow

Traditionally, we would start with a **Hello World** example. We’re not going to do that since we’re starting with the online version of Power Automate. Because flows are hosted in the cloud and intended to run standalone even when you are not there, the user interface options are much more limited. When running in the cloud, even if you could get a message box to pop up, who would see it?

Instead, we’re going to start with an example we’ve already carried out in *Chapter 6* – getting information from an online API. You’ll remember this as a simple GET request that returns information about a project hosted in GitHub. Our flow will have three steps:

1. A trigger to start the flow.
2. An HTTP request action to get the information about the GitHub project.
3. A create file action so we can save the results and view them later.

To start building this flow in Power Automate, follow these steps:

1. Click on the **Create** button. You’ll be shown a choice of flow types, as in *Figure 8.3*:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_213_0.png)

Figure 8.3 – Creating a Power Automate flow

2. We’re going to pick **Instant cloud flow** since it is the most simple – our flow will start at the touch of a button.

### What are the other flow types?

The other flow types are worth exploring later – an **Automated cloud flow** option is started by a trigger such as receiving an email, or a new document being updated to SharePoint. **Scheduled cloud flow** runs every X minutes/hours/days. **Desktop flow** we’ll meet later in the chapter, and **Process advisor** makes suggestions to streamline more complex flows.

3. Once we’ve picked **Instant cloud flow**, we’ll then get a choice of different manual triggers, as in Figure 8.4 (again, worth exploring in your own time). Here, we’ll give our flow a name, then choose the **Manually trigger a flow** option, and then click on the **Create** button:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_214_0.png)

Figure 8.4 – Selecting how to trigger a flow

4. Power Automate will create our flow, and bring us to the main flow editing screen, as in Figure 8.5. The first step in our flow (the manual trigger) is already created for us. This screen also gives us options to change the flow name (if we didn’t in the previous step), to undo/redo editing steps, to save our flow and flow checker, and test buttons that we’ll use later.

6. Power Automate will create our HTTP action. In Figure 8.7, we’ve already entered the settings for this action – not surprisingly, they are the same as we’ve used in Chapter 6 – a GET method and a URI of https://api.github.com/repos/OfficeDev/office-js. The other values we can leave blank.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_216_0.png)

Figure 8.7 – Second step in the flow – HTTP Connector

7. Even though our flow is now longer, there will still be a New step option at the bottom. Click on it, then search for the Create file option from OneDrive. We won’t show the search process this time; if you have the right action, it will have an icon and appearance similar to Figure 8.8:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_216_1.png)

Figure 8.8 – Settings for creating our file

8. In this action, we select the folder path – clicking on the Folder Path field will let you choose a path that already exists in OneDrive. Note, if you’ve just created a new username for Power Automate, you may need to sign in to OneDrive with this username and password first. We chose a general Downloads folder, but any existing folder will do.
