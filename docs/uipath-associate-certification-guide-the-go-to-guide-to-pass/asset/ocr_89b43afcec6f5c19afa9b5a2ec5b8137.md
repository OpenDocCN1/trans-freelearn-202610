In this section, we have explored how to install the UiPath.Mail.Activities package and have verified the installed dependency. We have also seen a high-level introduction of each activity and we will explore all the important email activities in upcoming sections.

# # Extracting email contents using Receive Messages

In this section, we will learn how to automate receiving emails, where the robot will read the contents of an email in a Google (Gmail) account. We will be using IMAP to achieve this goal. Assume the email address and password are already passed as an argument within the workflow and we consider only the first five unread email messages:

+   1. **Create a process**
+      Create a new process named `Email_Automation` in UiPath Studio. To proceed with this development, you need to send a sample email to the Gmail account.
+   
+   2. **Send email**
+      Send one test email to the Google email that needs to be automated, as shown in Figure 12.13:

![](bbox=[0.112, 0.522, 0.856, 0.752])

Figure 12.13 – Sample email