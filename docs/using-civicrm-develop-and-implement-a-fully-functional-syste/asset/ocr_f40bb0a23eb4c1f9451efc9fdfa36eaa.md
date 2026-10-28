# Sending e-mails

Having addressed core configuration and contact-related settings, we now turn our attention to e-mails generated from the system. CiviCRM may be configured to automatically generate e-mails for end user actions, such as registering for an event or making a donation. Additionally, administrators can send e-mails to one or more contacts using activities or CiviMail, the mass e-mail distribution tool. Though you are not required to use e-mail sending capabilities in order to use CiviCRM, you will miss out on a significant functionality if you choose not to.

First, set up your **Outbound Email** configuration at **Administer | Configure | Global Settings | Outbound Email (SMTP/Sendmail)**, and then choose **mail()**, **sendmail**, or **SMTP**-based mailing mechanisms. **SMTP** may provide more complete bounce headers for return mail processing, though the default **mail()** option is generally sufficient. Your hosting provider may also have a preference, or may only support certain options. Regardless of which option you choose and configure, be sure to use the built-in testing function and confirm receipt of the e-mail. This quickly and easily ensures that the outbound e-mail is working.

> If you plan to use CiviCRM for large broadcast e-mails, be aware of the responsibilities involved in maintaining a good sender reputation with ISPs and organizations that maintain whitelists/blacklists. Also be aware of the potential server load caused by large mailings. You may want to consider a third-party SMTP provider if you plan to use CiviMail on a large scale.

Once you have tested outbound e-mail, configure the settings at **Administer | Configure | From Email Addresses**. When sending e-mails from CiviCRM, the logged in user's e-mail will be the default FROM address. However, you may configure additional addresses available to the system and select them while sending an e-mail.

For example, your organization might have service-based generic e-mails, such as `events@my.org`, `questions@my.org`, and `membership@my.org`. Depending on your hosting setup, the FROM e-mail addresses used may need to exist in your mail server as mailboxes or forwards.

# Handling return e-mail traffic

The CiviMail component provides important functionality for processing the inbound e-mails. These e-mails are received and processed by the system for three purposes: