**Domain Information** is used to configure the name of your organization, the **FROM Email Address**, mailing address, and other contact details. The settings here are important as they will be used as the default FROM address for system-generated and CiviMail e-mails sent from CiviCRM. Note that the e-mail address used needs to be a valid user on your hosting server in order for CiviCRM to send e-mails properly (this requirement will depend on your hosting environment configuration).

If you are planning to use CiviMail for broadcast e-mail capabilities from your site, be sure to complete the domain address fields so that it can be included in bulk e-mails. This is a good practice for any legitimate broadcast e-mailer, and is required by anti-spam legislation in some jurisdictions (for example, the American CAN-SPAM Act), and CiviMail itself.

# # Viewing and Editing Contacts

The next configuration section, **Viewing and Editing Contacts**, handles settings related to contacts, including the contact record page, search settings, contact types, and address settings. We will walk through each of these areas.

**Site Preferences** controls what content areas are visible in different parts of the system. For example, if you are not planning to use tags as a categorization tool, you should remove it from your views. Don't need to have immediate access to the changelog (which records a running list of when edits to a contact record was made, with the name of the user and timestamp)? Remove it from your views.

The preferences are broken into four areas:

+   - **Viewing Contacts**: Controls the tabs that appear across the top of the contact record.
+   - **Editing Contacts**: Controls elements that will appear in the contact edit view (accessed from the contact summary tab).
+   - **Contact Search**: This advanced search tool groups fields by their type. You expand a group panel in order to access its respective fields. Use the options here to disable panels you don't want to be displayed.
+   - **Contact Dashboard**: CiviCRM has a special page, which can be exposed to logged-in website users called the **Contact Dashboard**. The dashboard summarizes the user-related records, including their membership history, event registration history, donation history, and pledge history. In addition, users may see related contacts that they are permissioned to view.

As you initially configure this section, you may be uncertain what options to select. Remember that you can always return at a later time to modify your settings. You will find whether you need or don't need certain elements listed here as you progress through the site implementation and begin working in the system.