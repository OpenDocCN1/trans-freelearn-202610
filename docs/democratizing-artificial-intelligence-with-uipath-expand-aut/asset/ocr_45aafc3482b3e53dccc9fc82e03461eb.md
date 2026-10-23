17. In between the `Ask for more Help` and `More Help` steps, click on the gear icon to add a condition, adding the following:

+   - **Condition:** `@more_help=="Yes"`

18. Drag a connecting arrow from the `More Help` step to the `Self-Help` step, as illustrated in the following screenshot:

![](bbox=[0.116, 0.235, 0.851, 0.557])

*Figure 10.14 – Completed UiPath_Flow process*

With the automation flow complete, we have one last step before we can move on to building the UiPath automation. We need to connect the Welcome flow with the UiPath flow.

19. Navigate back to `first-welcome-flow`.

20. Edit the `Confirm email` step, setting the following attributes:

+   - **Subflow:** `UiPath_Flow`

And that's it! We have successfully created a chatbot flow with Druid that connects to UiPath. In subsequent sections, we will build the UiPath automation component of this use case, and then test out the functionality.