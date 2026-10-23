# ## Building the Main workflow

Every automation project starts with a **Main** workflow. The purpose of the **Main** workflow is to kick off the project, invoking other workflows in the process.

From our future state design, the first step of the use case is retrieving emails from our inbox—we can recreate this by adding a folder to Outlook, named `Email Classification`, and having automation fetch emails from that folder. Proceed as follows:

+   1. Open the **Data Input** sequence.
+   2. After **Assign**, drag a **Get Outlook Mail Messages** activity into the workflow, setting the following attributes:
+      - **MailFolder**: `Inbox/Email Classification`
+      - **MarkAsRead**
+      - **OnlyUnreadMessages**
+      - **Top**: `1`
+      - **Output**: `list_emails`
+   3. Drag an **If** statement into the workflow, setting the condition to `List_emails.count > 0`.
+   4. Within the **Then** sequence of the **If** statement, drag an **Assign** activity into the workflow.
+   5. Within the **Assign** activity, set **NewTransaction** to `True`.

After adding the **Assign** activity within the **If** statement, our **Data Input** sequence should look similar to this: