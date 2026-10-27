6. To do this, we are going to mix regular text and a dynamic content field. First, add the hardcoded text that was just listed. Then, using the dynamic content variable listing popup, select the Number variable. You may need to expand the Alert – Get incident listing in this popup to see all the available variables.

7. Once you are done, you should see the following:

![](bbox=[0.11, 0.21, 0.85, 0.49])

Figure 12.26 – Posting a message completed

8. Once you are all done, click on the Save button in the Logic Apps Designer page header to save this playbook. If there are any errors, you will be informed of them. If not, the playbook will be saved. You will notice that the Save button in the header is grayed out until new changes are made.

9. As a final step, you need to attach this playbook to an alert to verify that the code is working as designed. Refer to Chapter 7, Creating Analytic Rules, for instructions on how to do this.

There you have it! A fully functional and incredibly useful Microsoft Sentinel playbook with no coding required. You just needed to figure out which fields to use where. This was a very simple example but, hopefully, it has given you some idea of what to use playbooks for.