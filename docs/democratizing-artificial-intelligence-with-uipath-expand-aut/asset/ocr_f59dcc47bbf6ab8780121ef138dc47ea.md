After Ask for more Help, we should have one final message, either thanking the user if they say no to any more help, or navigating them back to the beginning of the flow if they do ask for more help.

+   14. After Ask for more Help, add a child step with the following attributes:
+   
+   - **Step name:** Closing
+   - **Type:** Message
+   - **Message:** Okay! Have a great day
+   - **IsLastStep:** True

+   15. In between the Ask for more Help and Closing steps, click on the gear icon to add a condition, adding the following:
+   
+   - **Condition:** `@more_help=="No"`

The following screenshot illustrates this:

![](bbox=[0.133, 0.432, 0.86, 0.643])

Figure 10.13 – Adding a condition to Closing

The purpose of this condition is to only route the conversation to the Closing step if the user's choice was No for additional help. If the user selects Yes, then they should be rerouted to the beginning of the flow.

+   16. After Ask for more Help, add a child step with the following attributes:
+   
+   - **Step name:** More Help
+   - **Type:** Message
+   - **Message:** Okay!