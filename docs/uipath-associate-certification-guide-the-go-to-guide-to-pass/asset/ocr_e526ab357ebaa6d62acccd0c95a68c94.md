![](bbox=[0.316, 0.094, 0.648, 0.179])

Figure 12.17 – Get Password activity

+   II. Create the following variables to store the email ID, password, and actual emails. Enter the email address to be automated in the **Default** field, as shown in Figure 12.18:

| Name | Variable type | Scope | Default |
| :--- | :--- | :--- | :--- |
| email | String | Sequence | "processadminhr@gmail.com" |
| password | String | Sequence | Enter a VB expression |
| Emails | List<MailMessage> | Sequence | Enter a VB expression |

Figure 12.18 – Variables for the process

+   III. Click on the Get Password activity from Step I, enter the password copied in Step 4 of the previous section to the Password (1) field in Properties, and store the result in a variable named password (2), as shown in Figure 12.18:

![](bbox=[0.193, 0.506, 0.778, 0.691])

Figure 12.19 – Get Password properties

+   IV. Drag the Get IMAP Mail Messages activity to the workflow, as shown in Figure 12.19:

![](bbox=[0.292, 0.781, 0.672, 0.846])

Figure 12.20 – Get IMAP Mail Messages activity