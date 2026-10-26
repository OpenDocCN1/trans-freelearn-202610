# Learning about the flow debug window

Debugging from Flow Builder can be done with simple clicks. We do not need to create debug logs using trace flags and then go through **debug log details.**

**What are debug logs?**
A debug log records database operations and system processes such as validations, assignment rules, and so on, and any errors while executing a transaction. The system generates a debug log for a user to execute a transaction. You can specify a trace flag with start and expiration dates that contain the transaction's start time. Debugging logs gives greater details, and you need to have a good level of technical skills to be able to interpret these logs.

Flow debug is not just to be used when we run into issues. This is an extremely helpful tool for us to test chunks of functionality and make sure it works end to end as desired. The debug window captures details that let us see how our flow flows. This provides step-by-step results on how well the flow has been executed—how it is triggered, which actions are taking place, and the outcome. We can fine-tune our steps in the flow and fix any faults, optimize individual steps, customize error messages, add information messages, and so on.

Let’s look at the key features of the debug window.

The following are the steps from start to finish:

+   1. To access flow debug, open the flow in Flow Builder.
+   2. Click **Debug**.
+   3. Set the debug options and input variables.
+   4. Scroll through screens (if you have multiple screens) by clicking **Next**, and end the flow by selecting **Finish** on the last screen.
+   5. You can debug the flow again with the same input values or by changing input values:
+      A. **Change Inputs**: Select this to debug the flow with different scenarios (input values).
+      B. **Run Again**: Select this to rerun with the same inputs.

# Simple practical scenario – capture shipping address

The analyst will be able to capture the shipping address using **Screen Flow**.

Let’s walk through a happy path scenario. In this scenario, everything works magically as desired. Make sure we enter valid values so that our flow works from start to finish.

Let’s review the scenario that we discussed in *Chapter 6*.