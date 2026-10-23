With Retrain_Model.xaml complete, we can focus on connecting it with Main.xaml and fetching emails from the Retrain-Spam and Retrain-NotSpam folders:

+   1. Open Main.xaml and open the **End Process** sequence.
+   2. Drag a **Get Outlook Mail Messages** activity into the workflow, setting the following parameters:
+      - **MailFolder**: "Inbox\Email Classification\Retrain-Spam"
+      - **Top**: -1
+      - **Output**: list_retrainSpamEmails
+   3. Drag another **Get Outlook Mail Messages** activity into the workflow, setting the following parameters:
+      - **MailFolder**: "Inbox\Email Classification\Retrain-NotSpam"
+      - **Top**: -1
+      - **Output**: list_retrainNotSpamEmails
+      With our emails extracted, we can now pass them to our newly created Retrain_Model workflow, but we only need to run this workflow if we have any emails.
+   4. Drag an **If** activity into the workflow, setting the condition to (list_retrainSpamEmails.Count > 0) OR (list_retrainNotSpamEmails.Count > 0).
+   5. Within the **If** activity, drag an **Invoke Workflow File** activity into the workflow, setting the following parameters:
+      - **Workflow file name**: "Retrain_Model.xaml"
+      - **in_listSpamEmails**: list_retrainSpamEmails
+      - **in_listNotSpamEmails**: list_retrainNotSpamEmails