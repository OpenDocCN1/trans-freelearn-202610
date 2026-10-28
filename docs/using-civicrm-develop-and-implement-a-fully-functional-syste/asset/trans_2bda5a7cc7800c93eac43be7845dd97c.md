

All four approaches will bring up the following form for e-mailing via CiviCRM:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_245_0.png)

While it seems like a fairly standard e-mail form, let's go through it field-by-field to learn about its special features:

- **From**: This defaults to the primary e-mail of the currently logged in user but also allows you to select from other e-mails set up in **Administer | Configure | FROM Email Addresses**. This is particularly useful for situations where there are departmental or functional e-mails (for example, `info@example.org`, `volunteering@example.org`, `events@example.org`) or where one staff member sends e-mails on another's behalf (for example, `ExecutiveDirector@example.org`). The e-mail will be logged as an activity for all contacts specified in the **From, To, CC, and BCC** fields.
- **To**: The primary e-mail address of the contact(s) to be e-mailed. If you arrived at this form through the contact record or search results, the field will be populated with the originating contact(s) e-mail address. If you arrived using the **Create New** button, you will need to search for the contact(s) you wish to send to. You can add additional e-mails using the standard auto-completion search functionality (name and e-mail).
- **Add CC | Add BCC**: These links provide auto-completion search fields and ensure that the e-mail activity is added to the record of the relevant contacts.
- **Subject**: In addition to providing text that will appear unchanged on the subject line in people's inboxes, you can use the Insert Token link to include information from a contact's record that should be looked up and inserted into the subject line. This can include either core fields such as a person's first name, or contact-type custom fields that are specific to your installation of CiviCRM. The latter are included in the alphabetized drop-down list of field names as **Custom Field Name :: Custom Data Group Name**.
- **Use Template**: E-mail templates are used in slightly different ways in different parts of CiviCRM:
  - Unlike automated message templates generated from event signups, contribution pages, and other public facing forms, templates selected when manually sending an e-mail will pre-populate the content field and provide you an opportunity to manually edit the content before it is sent out. As a result, the process is more like making and editing a copy of a Word file than auto-generating a letter from a Word mail merge template.
  - Since it populates the field and then provides access to further edit the content, you can take advantage of the tool by creating e-mail templates containing just your design/layout branding, and also use it to create standardized responses for common queries received by phone or mail. When creating your template, take the time to test it thoroughly and ensure cross-client compatibility, including reviewing how it will appear in web-based e-mail tools (Gmail, Hotmail, and so on). It can take significant fiddling to get HTML-formatted e-mails to look consistent in different e-mail clients, especially when you are trying to do sophisticated formatting. By investing the time to create a well-constructed template, you will save time for end users creating the e-mails and help ensure effective visual branding.
- **HTML Format**: CiviCRM ships with two popular WYSIWYG (what you see is what you get) editors which support standard formatting tools when constructing your e-mail. We find that Drupal users will typically use **CKEditor**, and Joomla! users will select **TinyMCE**, as they are more commonly used in those respective CMSs. Note that in CiviCRM v3.3, Joomla! users will be able to use the default or user-specified editor installed in Joomla! and selected in **Global Configuration**, which will significantly extend the options available. The following screenshot refers to CKEditor. Some standard caveats apply:
- While visually well-designed HTML e-mails tend to improve readability, click-through, and response rates, it is important to test and periodically retest how the e-mails are appearing in popular desktop and web-based e-mail clients. Invalid HTML markup, such as unclosed tags are red flags to spam checking software; so beware of inadvertently creating deliverability problems by having someone who is not familiar with HTML doing anything fancy in the HTML source view.
- CSS used in your e-mail should always be inserted directly in the HTML tags. Most e-mail clients don't support style declarations inserted in tags through class and ID values.
- Don't cut and paste directly from Microsoft Word because it uses non-standard markup code. Invisible to you when pasting, this markup code will often prevent your messages from appearing correctly in your recipients' inboxes because it references Microsoft-specific elements that are not widely supported. If you need to copy something from a Word document, the easiest way to remove the problematic markup code is to paste using the special Paste from Word or the Paste as Text icon. Paste as Word will attempt to retain structural elements, such as bullet lists, whereas Paste as Text more thoroughly strips all design and layout elements.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_247_0.png)

- For an image to appear in your constituents' browsers properly, it needs to be available on the Web somewhere. Generally, it is best to upload any image you need in your e-mail to a publicly accessible part of your website, making note of the URL that displays the image.
- When referencing pages on your website or inserting images, be sure to use the full URL (http://...), not a relative path. Remember that the e-mail received by your constituents will not be directly connected to your website, so relative path resources will be meaningless.
- There are a number of good resources (many of them free) for obtaining e-mail templates that are compatible with most e-mail clients. Consider obtaining some as the basis for your template and then modify to suit your organizational branding.

- 3. Select **Mailing Labels** on the **- actions -** button.
- 4. Click on the **Go** button, and then configure mailing label options as follows.
- 5. Beside **Select Label**, choose the Avery label type that you will be printing on (typically indicated on the packaging). If you are not using an Avery product, look for an indication showing which Avery number the labels are compatible with, as many companies use the same dimensions. You can also look up the dimensions for the available labels on the Avery site for your country (for example, http://www.avery.com site does not list the A4-sized products that begin with L, and http://www.avery.fr does not list the North American 8.5 x 11 ones).
- 6. Choose the mailing **Location** for each contact. Every contact must have one of its addresses marked as **Primary**.
- 7. Choose whether you want to remove contacts with the "Do Not Mail" communication preference selected.
- 8. Depending on how you have decided to enter and store your data in CiviCRM, you may want to reduce postage charges either by choosing to merge labels for contacts belonging to the same household, or by choosing to merge labels for all contacts with the same address (a common need for organizations that do not use **Household** contacts).
- 9. Click on **Make Mailing Labels** and save the resulting PDF download file to your hard drive.
- 10. Print the file. If you have a big printout, you might want to initially print only one page to check the alignment of the text with the labels.

## Recording a postal mailing

Normally, when you print a list of mailing labels, you're about to communicate with those contacts. The process of generating mailing labels does not automatically record the fact that you will be using it for a mailing. As a best practice, we recommend creating an activity for your mailing recipients after generating mailing labels as an effective way to retain a history of communication. The default installation of CiviCRM does not include an activity type appropriate to postal mailings, so the first step is to create such a type. Here are the steps to record mailings at the same time mailing labels are printed:

- 1. Create a Postal Mailing Activity:
  - Click on Administer | Option Lists | Activity Types
  - Click on Add Activity Type
  - Enter a suitable **Label**, such as **Postal Mailing**
  - Enter a suitable **Description**, such as **Postal Mailing**

