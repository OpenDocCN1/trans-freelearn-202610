
## Understanding the current state of the use case

Before we venture into designing and building a use case, we need to understand the current state of the opportunity. With an increase in hiring in *Company ABC* has come an influx of service requests to the **information technology (IT)** team. Some of these service requests are simple requests that can be resolved with little human interaction, such as the following:

- Resetting passwords
- Upgrading colleagues' video conferencing software to the professional version

With the increased hiring of *Company ABC*, a lot of the support representative's time is spent resolving quick, simple requests. To increase the capacity of the support team, the IT team has been asked to automate these two requests.

Given that the tasks of resetting passwords and upgrading application licenses are straightforward, we can see that this automation opportunity has the following characteristics:

- **Repetitive in nature**: Support representatives must perform the same series of steps for each service request.
- **Time-consuming**: Completing each service request can take time away from support representatives performing more meaningful tasks.
- **Rules-based and structured**: Resetting a user's password or upgrading their license type is very rule-based.

Automation of these tasks can provide a great deal of impact for not only the IT team but also for colleagues within *Company ABC*. In the next section, we will focus on creating a future state design for implementing this IT helpdesk bot.

## Creating a future state design

With our new understanding of the current state and the pain points of the IT team, we can start by creating a future state design of the automation, as represented in the following diagram:

![](img/05268897c8ac5d80eb3fead1cc8a822a_318_0.png)

Figure 10.1 – Future state design of the IT helpdesk bot

In the future state design of the use case, we could have a human launch our Druid chatbot (which could be connected to Slack, Microsoft Teams, or via the web), and prompt the chatbot to help with either password resets or application upgrades. Once the chatbot receives the request, it can trigger UiPath automation with an input argument on the specific task (password reset or application upgrade).

Before we go into details on the future state design, it's important to know some of the basics of building a chatbot. All conversational flows contain intent and dialog, which are the building blocks of a chatbot. When we represent a conversation between a person and our chatbot, the dialog between the two will be based on some intent. This is outlined further here:

- **Intent:** A purpose for starting the conversation (also known as an utterance)
- **Dialog:** A conversation that is in response to an intent
- **Flows:** A combination of intents and dialog

All Druid chatbots are based on this methodology, whereby a user initiates a conversation with a chatbot (an utterance). Based on that utterance, the chatbot best matches with an intent that then starts a dialog, which could contain free text, buttons, or other inputs.

With this understanding, we can design the flows of our chatbot. By default, a chatbot should have the following flows configured:

- **Welcome flow**: A flow to welcome the user once they initiate the chatbot
- **Fallback flow**: A flow for the chatbot to fall back on if the bot does not know the answer or response to a user's input

In addition to leveraging the Welcome and Fallback flows, we will need to add one additional flow for our automation. This flow will be called UiPath_Flow, where we will provide a set of buttons for the user to choose from. Depending on the button the user chooses, Druid will trigger an automation with an argument to either reset their password or to upgrade their application license.

That concludes our future state design. In this section, we discussed chatbot basics, created a diagram to resemble our future state design, and introduced the three flows we will build in our chatbot. In the next section, we will get started with development by building the Druid chatbot.

## Building a solution with Druid

As we venture into building our defined use case, we'll start with building our chatbot component with Druid. For this use case, we'll start out with Druid's **Druid Starter** template to set the foundation of our chatbot. This template comes with a definition of system entities and common flows that every chatbot should leverage.

## Creating a project

To start a project with the Druid Starter template, complete the following steps:

1. In the main menu, click on the Bot dropdown and select AddNewBot.
2. Once the Create New Bot window appears, enter IT Help Desk Bot as the bot's name, choose English (United States) as the default language, and click Save. The process is illustrated in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_320_0.png)

Figure 10.2 – Creating a new bot

Now that the bot is created, let's import the Druid Starter template to get us started.

3. In the main menu, click on the Solution dropdown and select Import Solution.
4. Once the Solution Library page appears, select Druid Starter and then select Import Solution.

With the Druid Starter template installed, we have a good foundation for us to build off of. Let's get started with creating some conversational flows.

## Editing the Welcome flow

The first flow of the Druid Starter template is known as the Welcome flow. The purpose of this flow is to welcome the user when they first interact with a chatbot. If you navigate to the flows of the project (click Flows under Solution Contents), you will notice our Welcome flow, named first-welcome-flow, as illustrated in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_321_0.png)

Figure 10.3 – Welcome flow

Within `first-welcome-flow`, let's add our first prompt asking the user for their email address, as follows:

1. Within `first-welcome-flow`, click the **Edit** button to edit the flow.
2. Click **AddChildStep** within the parent message, as illustrated in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_321_1.png)

Figure 10.4 – Adding a child step

3. Once **newMessage** appears, click the **Edit** button to edit the child step, setting the following attributes:

- **Step name:** `Ask for Email`
- **Type:** `Prompt`
- **Step message:** `Please input your email address`
- **Input mapping:** `@email`

![](img/05268897c8ac5d80eb3fead1cc8a822a_323_0.png)

Figure 10.6 – first-welcome-flow

For now, that concludes the welcome flow. As we build out the other flows, we'll come back and make some small edits.

## Creating an automation flow

With our welcome flow complete, let's venture into the main flow of the chatbot: the automation flow. In this flow, we'll present the user with options to choose from, launching a UiPath automation based on the user's input. Let's get started, as follows:

1. Click **Create New Flow** to create a new flow, setting the following attributes:
   - **Name:** UiPath_Flow
   - **Description:** Main Flow for IT Help Desk Bot
   - **Utterances:** help
   - **Category:** Automation Flow

With the flow created, let's add a choice step to the flow, giving users the ability to choose between resetting their password or upgrading their license type.

2. Click + **Add new step** to create a new step, setting the following attributes under the **General** tab:

- **Step name:** `Self-Help`
- **Type:** `Choice`
- **Step message:** `What can I help you with?`
- **Input mapping:** `@self_help_choice`

3. Within the **Metadata** tab of the `Self-Help` step, set the following attributes:

- **BlockUserInput:** `True`
- **Is first step?:** `True`

Additionally, within the **Metadata** tab, you'll see an option to add choices (see *Figure 10.7*). These choices will be button options for the user to choose from when they interact with the chatbot.

4. Click +**AddChoice**, setting the following attributes:

- **Caption:** `Password Reset`
- **Value:** `pass_reset`

5. Click + **AddChoice**, setting the following attributes:

- **Caption:** `Upgrade AppY`
- **Value:** `upgrade_appY`

The following screenshot depicts the process of adding choices:
