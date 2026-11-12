Ensure the correct variable is assigned to **Attachments** so that the Robot can attach the correct document while sending emails, as shown in Figure 12.36:

![](bbox=[0.115, 0.15, 0.855, 0.415])

Figure 12.36 – Attachment of Send SMTP Mail Message

# 9. Output verification

Run the project using the **Run** button or *Ctrl + F5* to verify the output. Check the destination email to see if you received the two emails with attachments. Ensure the email you sent in *Step 7* is displayed as the output in your email, as shown in *Figure 12.37*:

![](bbox=[0.115, 0.57, 0.855, 0.735])

Figure 12.37 – Output verification in the destination email

In this section, we have automated sending emails using UiPath Robot and SMTP. We used a Gmail account to send an email with attached documents using the **Attachments** properties, and an Outlook account to receive the email. Finally, we executed the Robot to verify the output. In the next section, we will look at how to automatically process the attachments from an email.