

# 元数据

IsRepeater

BlockUserInput

Exception handler

Is WhatsApp template

AdvancedEditing

Choice[edit]

Preview

Password Reset

Upgrade AppY

Password Reset

Upgrade AppY

pass_reset

upgrade_appY

buttonClass

buttonClass

+ AddChoice

Is first step?

Is last step?

Figure 10.7 – Adding choices to the Self-Help flow step

+ 6. Click Save to save your choices.

Great! By clicking Save, we have added a choice that will look similar to this when we test it out. Note that by checking BlockUserInput, the user can only select from the two buttons and not add free text:

What can I help you with?

Password Reset

Upgrade AppY

Please select one of the options above

Figure 10.8 – Druid choices

With our choice step added, the next step is to add a UiPath connection.

+ 7. Use Druid's **Flow Diagram Designer** by clicking on the **Flow** button highlighted in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_326_0.png)

Once the **Flow Diagram Designer** appears, you should be able to see the `Self-Help` step we just added. Hovering over the + sign underneath the flow step will present several options to add as a child step, as illustrated in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_326_1.png)

+ 8. Add **UiPath Automation** by clicking on the **Ui** button.
+ 
+ 9. Within the `Launch Automation` step, set the following attributes:
+ 
- **Step name:** `Launch Automation`
- **Type:** `UiPath`
- **Technology:** `Automation: attended (Robot.JS)`
- **Manually set process information**
- **Process:** `IT_Helpdesk_Chatbot`
- **Step message:** `Launching UiPath Automation...`

+ 10. Within `Launch Automation`, set the following process **input/output (I/O)** arguments:
+ 
- **Name:** `in_Task`; **Direction:** `In`; **Druid entity name:** `@self_help_choice`
- **Name:** `in_Email`; **Direction:** `In`; **Druid entity name:** `@email`
- **Name:** `out_SRNumber`; **Direction:** `Out`; **Druid entity name:** `@SR_Number`

The process is illustrated in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_327_0.png)

Figure 10.11 – Launch Automation step details

The process is illustrated in the following screenshot:

Step name

Ask for more Help

Type

Choice

Step message

Is there anything else I can help you with?

Input mapping

@more_help

SkipIfKnown

Hide in conversation history

NER Context

Metadata

IsRepeater

BlockUserInput

Exception handler

Is WhatsApp template

AdvancedEditing

Choice

Edit

Yes

No

Figure 10.12 – Ask for more Help choice

13. Within Flow Diagram Designer, make sure to connect both the Error sending the request and After Job Complete (Error) flow steps to the newly created Ask for more Help choice.

After Ask for more Help, we should have one final message, either thanking the user if they say no to any more help, or navigating them back to the beginning of the flow if they do ask for more help.

+ 14. After Ask for more Help, add a child step with the following attributes:
+ 
- **Step name:** Closing
- **Type:** Message
- **Message:** Okay! Have a great day
- **IsLastStep:** True

+ 15. In between the Ask for more Help and Closing steps, click on the gear icon to add a condition, adding the following:
+ 
- **Condition:** `@more_help=="No"`

The following screenshot illustrates this:

![](img/05268897c8ac5d80eb3fead1cc8a822a_330_0.png)

Figure 10.13 – Adding a condition to Closing

The purpose of this condition is to only route the conversation to the Closing step if the user's choice was No for additional help. If the user selects Yes, then they should be rerouted to the beginning of the flow.

+ 16. After Ask for more Help, add a child step with the following attributes:
+ 
- **Step name:** More Help
- **Type:** Message
- **Message:** Okay!

17. In between the `Ask for more Help` and `More Help` steps, click on the gear icon to add a condition, adding the following:

+ - **Condition:** `@more_help=="Yes"`

18. Drag a connecting arrow from the `More Help` step to the `Self-Help` step, as illustrated in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_331_0.png)

*Figure 10.14 – Completed UiPath_Flow process*

With the automation flow complete, we have one last step before we can move on to building the UiPath automation. We need to connect the Welcome flow with the UiPath flow.

19. Navigate back to `first-welcome-flow`.

20. Edit the `Confirm email` step, setting the following attributes:

+ - **Subflow:** `UiPath_Flow`

And that's it! We have successfully created a chatbot flow with Druid that connects to UiPath. In subsequent sections, we will build the UiPath automation component of this use case, and then test out the functionality.

# Building an automation solution

With our chatbot built, we can shift our focus to creating automation workflows for the use case. Approaching this use case, we will build our project in a couple of steps, as follows:

+ 1. Creating a project
+ 2. Building `Reset_Password` and `Upgrade_App` workflows
+ 3. Building the **Main** workflow

After completing these steps, we will have the foundation of our project complete and ready for testing.

# Creating a project

For this project, we'll create a new blank project in Studio. To start a project, complete the following steps:

+ 1. Navigate to the **Start** tab of UiPath Studio.
+ 2. Choose **Process**.
+ 3. Name your project `IT_Helpdesk_Chatbot`, and provide a folder location to save the project to.
+ 4. Click **Create** to create a project and to get started.

That concludes creating a project. We'll revert to the **Main** workflow in a later section where we build out the workflow, but for now, we'll transition into building supplemental workflows.

# Building Reset_Password and Upgrade_App workflows

With an automation project created, we can focus on creating some of the supplemental workflows that'll become building blocks of our **Main** workflow. Because we don't have actual accounts to reset or an application license to upgrade, our objective is to create modular workflow sequences that can simulate password resets or application license upgrades.

To start, let's create a `Reset_Password` workflow, as follows:

+ 1. In the `IT_Helpdesk_Chatbot` project, create a new workflow file called `Reset_Password`.
+ 2. Once the new workflow is created, create two arguments, as shown in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_333_0.png)

Figure 10.15 – Reset_Password arguments

The purpose of each argument is this:

+ - `in_Email`: To input an email address from the chatbot
+ - `out_SRNumber`: A random SRN that we will output in the chatbot

Once the arguments are added to the workflow, we can start adding activities to the blank canvas.

+ 3. Add a **Message Box** activity, with `"Resetting Password for"+in_Email` as the message and `Info` as the log level. This will output a message box, simulating the steps that automation could take if we were connected to a system that could reset passwords.
+ 
+ 4. After the **Message Box** activity, add an **Assign** activity with the following parameters:
 - **To:** `out_SRNumber`
 - **From:** `"SR"+Right(Now.Millisecond.ToString,2).ToString`

With the **Assign** activity added and using **Right** to generate an almost random number, we have a workflow that looks like this:

![](img/05268897c8ac5d80eb3fead1cc8a822a_333_1.png)

Figure 10.16 – Reset_Password workflow

That concludes the Reset_Password activity. Let's now focus on the Upgrade_App activity.

+ 5. In the IT_Helpdesk_Chatbot project, create a new workflow file called Upgrade_App.

+ 6. Once this new workflow is created, create two arguments, as shown in the following screenshot:

| Name | Direction | Argument type | Default value |
| in_Email | In | String | Enter a VB expression |
| out_SRNumber | Out | String | Default value not supported |
| Create Argument |  |  |  |

Figure 10.17 – Reset_Password arguments

The purpose of each argument is this:

+ - in_Email: To input an email address from the chatbot

+ - out_SRNumber: A random SRN we will output in the chatbot

Once the arguments are added to the workflow, we can start adding activities to the blank canvas.

+ 7. Add a Message Box activity, with "Upgrading App for"+in_Email as the message and Info as the log level. This will output a message box, simulating steps that automation could take if we were connected to a system that could reset passwords.

+ 8. After the Message Box activity, add an Assign activity with the following parameters:

+ - To: out_SRNumber

+ - From: "SR"+Right (Now.Millisecond.ToString,2).ToString

With the Assign activity added and using Right to generate an almost random number, we have a workflow that looks like this:

