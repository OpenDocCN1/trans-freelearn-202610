![](bbox=[0.11, 0.1, 0.86, 0.28])

Figure 6.6 - A record-triggered flow – Configure Scheduled Paths

Note
When you create autolaunched flows, the Screen element is not available for us to add on the canvas. Autolaunched flows run in the background and user interaction is not needed, so we do not need a screen element.

To see our record-triggered flow in action, let us take a simple scenario.

# A business scenario for a record-triggered flow

In this scenario, a sales analyst will get a reminder task created immediately and after three days of no activity on the opportunity, if 1) a high-value opportunity reaches a certain stage, or 2) the opportunity belongs to a premier customer. If there is no activity for seven days, escalation is sent to the account owner.

Let’s see a record-triggered flow in action. To create it, we select the Opportunity object that triggers the flow, then we configure the trigger, set entry conditions with condition logic, and select the option to run the after-record save. In the following scenario, we create two tasks using Create Records elements and an escalation using the Email Alert action: