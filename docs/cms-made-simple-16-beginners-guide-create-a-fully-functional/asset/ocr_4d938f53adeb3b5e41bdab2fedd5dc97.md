Use the functionality for the Admin Notifications already implemented in the module, so that the support staff are informed if a new ticket is opened or a new reply is added.

In the admin area of the module Calguys Feedback Module (tab Fields), you can create a new custom field that allows one of the following values: Opened, Waiting for Response, or Closed. Use the field type Dropdown to define it. This field can be set by the support staff while working on the ticket and shown in the summary view of the tickets, so that the customer can see what the current status of the ticket is.

You have seen how you can use the module that is not dedicated for creating a service desk to set up the required features. Be open-minded and experiment with the modules. Even if their description is not exactly what you need, you can use it in your way, as shown in this section.

# Pop quiz – creating your own functionality

+   1. Why should the Allow users to login more than once box be selected when setting preferences in the FrontEndUsers module?
+      a. When the user is currently logged in and the session expires in the browser and not on the server, the user would not be able to log in again.
+      b. You should give users with a dual personality a chance to log in at least twice at the same time.
+      c. A lot of users are working on different computers (private, office, and so on), so they can log in from more than one computer.
+      d. When this box is not checked, it is not possible to generate a new password (or regenerate the username) when the user once forgets it.
+   2. What is the right statement according to the following Smarty tags used in protected pages?
+      {CGFeedback key1="Ticket" key2=$userid}
+      {CGFeedback key1="Ticket" key2=$userid action="summary"}
+      a. These tags show the summary view of all comments written by all registered users.
+      b. These tags show the comment form and the summary view of all comments written by one user.
+      c. These tags show the summary view of all comments written by all users, who are currently visiting the website.
+      d. These tags show the comment form and the summary view of all summaries of professional books written by all users having the same ID.

```
{CGFeedback key1="Ticket" key2=$userid}
{CGFeedback key1="Ticket" key2=$userid action="summary"}
```