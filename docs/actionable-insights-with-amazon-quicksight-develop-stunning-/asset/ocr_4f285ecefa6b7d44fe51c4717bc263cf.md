Both can help give your users context. Title and description can be particularly useful in
scenarios where there are many sheets. Navigation actions can help you define actions that
will allow your users to quickly navigate to different sheets within the same analysis.

When configuring navigation actions, you will need to choose a visual in your analysis
and then provide the following information:

+   - **Action name**: The name of the navigation action.
+   - **Activation**: **Menu option** or **Select**: **Menu Option** will add a menu item when you
+   click a data point in your visual. This option, when clicked, will activate the URL
+   action. On the other hand, with the **Select** option, your URL action will be activated
+   directly when you click a data point.
+   - **Target sheet**: The sheet name where your user redirects to when the action activates.
+   - **Parameters**: This lets the user set parameter values after navigating to the
+   target sheet.

Next, we will look at another type of action: URL actions.

# # Working with URL actions

URL actions allow us to redirect our user to another website or another dashboard when
the user clicks on a specific area of a dashboard. URL actions can help your BI users easily
retrieve additional context for the dashboard.

When configuring URL actions, you will need to choose a visual in your analysis and then
provide the following information:

+   - **Action Name**: A user-friendly and descriptive name for the URL action.
+   - **Activation**: **Menu Option** or **Select**: **Menu Option** will add a menu item when you
+   click a data point in your visual. This option, when clicked, will activate the URL
+   action. On the other hand, with the **Select** option, your URL action will be activated
+   directly when you click a data point.
+   - **URL**: The URL where you want to redirect your user.

> > **Note**
> > You can add references to the parameters of field values in the URL field.
> > This will allow you to redirect your users to different URLs, based on the user
> > selection. The syntax to refer to a parameter is <<$parameter-name>>,
> > while the syntax to refer to a field is {{ {field-name} }}.