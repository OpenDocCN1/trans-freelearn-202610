Unlike the canary release pattern, the dark launch pattern is not a blue-green deployment
that depends on the infrastructure but is implemented in the application code. To set
up the dark launch pattern, it is necessary to encapsulate the code of each feature of the
application in elements called **feature flags** (or **feature toggles**), which will be used to
enable or disable these features remotely.

We will look at the implementation and use of feature flags with an open source
framework and a cloud platform in the last few sections of this chapter.

In this section, we have presented the practice of blue-green deployment, along with
its concepts and patterns, such as the canary release and dark launch patterns. We have
discussed that this practice requires changes to be made in the production infrastructure
since it’s composed of two instances of the infrastructure—one blue and one green—as
well as a router that redirects users’ requests.

Now that we’ve talked about blue-green deployment patterns, we will look at how to
implement one in practice in an Azure cloud infrastructure.

# # Applying blue-green deployments on Azure

Now that we’ve looked at blue-green deployment, we’ll look at how to apply it to an Azure
infrastructure using two types of components—App Service slots and Azure Traffic
Manager.

Let’s start by looking at the most basic component—App Service slots.

# # Using App Service with slots

If we have an Azure subscription and want to use blue-green deployment without
investing a lot of effort, we can use App Service slots (Azure Web Apps or Azure
Functions).

In Azure App Services such as a Web App, we can create a second instance of our Web
App by creating a slot for it (up to 20 slots, depending on the App Service plan). This slot
is a secondary web app but is attached to our main web app.

In other words, the main web app represents the blue environment, and the slot represents
the green environment.