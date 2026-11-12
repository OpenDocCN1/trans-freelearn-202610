Let's try some of these options with a simple example. We are given the following string:

> "You have an amazing opportunity to connect with one of the celebrities to ask questions. Please reach out to John.Doe@mail.com or Jane.Doe@mail.com to reserve your seat today. For more information, reach out to 5992754159."

We need to use and extract all the email addresses available in the string. To do this, proceed as follows:

+   1. Open UiPath Studio and create a new **Blank Process** activity.
+   2. Open the Main.xaml file in the **Designer** panel to start building your workflow.
+   3. Place a **Sequence** activity in the **Designer** panel.
+   4. Create a new string variable named InputString to hold the input string given to us.
+   5. Place an **Assign** activity within the **Sequence** container and assign the InputString variable The string given above in the block section.
+   6. Search for the **Matches** activity from the **Activities** panel and drag and drop it below the **Assign** activity.
+   7. Configure the Input property of the **Matches** activity by providing the InputString variable name. The Input property provides the string with the regex applied.
+   8. Click on the Configure Regular Expression button on the **Matches** activity to open the RegEx Builder window.
+   9. Copy the input string that we want to use and paste it in the **Test Text** section of RegEx Builder.
+   10. Select the regex type as **Email** from the **Expression Type** dropdown. Once you select the type, you can see the prebuilt regex for emails that show up in the **Full Expression** section. Further, it will highlight the two email addresses we have in the **Test Text** section.
+   11. Configure the **Result** property of the activity by creating a new variable named **EmailList** using the **Ctrl + K** variable creation shortcut.
+   12. Search for a **For Each** activity and place it below the **Matches** activity.
+   13. Configure the **Type Argument** field of the **For Each** activity by searching for System.Text.RegularExpressions.Match as the type of each item.
+   14. Place a **Write Line** activity inside the loop and configure it with the **item**.ToString code to print the matching email address of the current iteration.