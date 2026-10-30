| Feature/Action group | Outlook | Exchange Server | Email |
|---|---|---|---|
| Connection capability | Any mailbox that is connected through Outlook, including Outlook 365 | One personal or shared mailbox | Any IMAP server |
| Retrieve email messages | Yes | Yes | Yes |
| Send email messages | Yes | Yes | Yes |
| Process email messages | Yes | Yes | Yes |
| Save messages | Yes | No | No |
| Respond to messages | Yes | No | No |

Table 11.1 – A comparison between different email options in PAD

As we can see here, there are many similarities between the different action groups and one major difference. While the **Exchange Server** and **Email** action groups work with network requests to execute the actions, the Outlook actions address the functionality of a locally installed Outlook client. This has the following consequences:

+   - When using Outlook, a local program is started; for example, this program must also be available, installed, and set up.
+   - The advantage is that the actions can be used with all mailboxes that Outlook is set up for, including Outlook 365, local Exchange Server, or any other mailbox.