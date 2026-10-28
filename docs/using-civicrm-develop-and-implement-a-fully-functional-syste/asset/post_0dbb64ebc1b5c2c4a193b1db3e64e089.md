
## CiviCRM Basics: Moving through the System and Working with Contacts

The default summary tab is perhaps the most important tab, as it tracks the contact name, communication details and preferences, addresses, and demographics (if it is an individual contact type). Click on the Edit button at the top of the tab to view all the available fields for your contact. You will notice that the edit screen is broken into grouped panes of fields corresponding to the different types of information you are collecting.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_156_0.png)

### Contact details

The contact details pane contains basic information about an entity, including the following:

- Contact name, including the nickname, which may be useful as an alternative value for searching. For example, if the name of a certain not-for-profit organization in your database typically goes by an acronym, you may wish to spell out the full name for formal purposes and place the acronym in the Nick Name field. It can then be easily exported with the record and found when using the search bar.
- Notice that if any contact subtypes have been configured, they will be available for selection in this area (Contact Type drop-down). Once you've selected a subtype and saved data to any of the custom fields associated with the subtype, the ability to change the type will be locked. Only those subtypes corresponding to the primary type are listed. For example, if you are looking at an individual record, only the individual subtypes will be available.
- Individual records have a special Current Employer field that creates a relationship between the individual and organization with whom they are employed. The current employer field is displayed prominently in the contact summary tab and can easily be exported with the individual record. Note that if you enter the name of an organization that does not exist in this field, an organization contact record will be created. You can take advantage of this to reduce the data entry.
- Communication options include e-mail, phone, instant messenger, website, and OpenID. Each of these fields allows you to add multiple values and categorize them. For example, a person may have a phone and mobile number for home, and a phone, mobile, and a fax number for work. Alternatively, someone may have a work e-mail and personal e-mail recorded with their record. If your site does not require tracking OpenID, you may remove it from the interface through Administer | Configure | Global Settings | Site Preferences.
- If multiple communication values are entered, one of them will be marked as the primary one. The primary value can be more easily exported, regardless of its type. For example, some of your contacts may wish to have their home phone marked as primary, while the others prefer their work or mobile to be marked as such. The primary flag is not concerned with the source or type of the record; it simply indicates which their preferred option is.
- The e-mail field has two additional checkboxes unique to its function — **On Hold** and **Bulk Mailings**. An e-mail address marked as **On Hold** indicates that it may be a faulty address. While you can manually mark an address as such, it is more likely that the field was flagged this way as a result of the CiviMail return mail processor. The mail processor is an optional tool that can be set up to retrieve any bounced (returned as undeliverable) e-mails and flag them as **On Hold**. This provides you with an opportunity to review the records and determine the source of the problem. While they are tagged as **On Hold**, they will be excluded from any future CiviMail mailings. The bulk mailings flag designates one e-mail address as the preferred mailing address for any bulk e-mails generated through CiviMail. For example, a person may want their work e-mail designated as the primary address as they want to receive direct communication immediately. However, weekly e-mail newsletters generated to all members of your organization should be sent to the person's home e-mail so as not to clutter their work inbox.
- The source field may be used to record how this particular contact came to be involved in your organization, or how they were added to the system. This may be particularly useful if your organization's growth is primarily due to word-of-mouth networking. For example, you might record that Jane Doe was introduced to the organization through outreach from one of your board members.
- External ID records a unique ID from some external source. If you are importing records from another database such as MS Access, you may want to import the key ID field for the contact into this field. While you won't typically use it once you are working exclusively in CiviCRM, it provides a useful way to trace the record back to its original source in the previous database system. This may also be used for data integrity analysis after the initial import process. In larger organizations with more complex IT infrastructure, it may enable easier on-going data synchronization with other software tools.

### Custom data

As we'll see in *Chapter 6, Communicating Better*, CiviCRM allows you to create sets of custom fields and indicate whether they should be displayed inline or in their own tab on the Contact form. If some custom inline fields have been defined for all contacts or the specific contact type you are editing, these fields will appear directly under the contact details pane.

### Address

Similar to communication options, you may create multiple address sets, categorize them (home, work, and so on), and flag one of them as the primary address. There are a few important things to note:

- An existing address may be flagged as the billing address or you may create a new address and use the **Location Type** option to designate it as a billing address. The former is useful if a single address is used for multiple purposes. The billing address will be used if the contact makes any online credit card payments for donations, events, or membership.
- Individual contacts have the option of choosing a household record's address for an address set. In order to do so, begin typing the household name and select it from the drop-down list. If the household record does not exist, you can create a new one by typing in the desired name of the household record. A contact may have only one associated household address.
- Each record may only have one address set for each address type. In other words, you can't designate three addresses as **Work** in a single contact record. This may seem somewhat limiting, as it would not be uncommon for an individual to have two work locations, or a home and second home address. However, the usefulness of this limitation becomes apparent when you export records, as you have the option of exporting a single address type with the record. Having multiple address sets with the same type would either require duplicate records or duplicate field columns, either of which will be difficult to work with. If you have the need for tracking multiple addresses of a similar type, consider adding option values labeled as **Home1**, **Home2**, **Home3**, and so on (**Administer | Option Lists | Location Types**).

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_159_0.png)

> When you first install CiviCRM, the address pane contains all of the available fields, including some you may not want to display. Visit **Administer | Configure | Global Settings | Address Settings** to turn off elements in this pane. For example, you may only want to track two address lines, and so you would disable **Additional Address 2**. Likewise, it is unlikely that you would need to manually edit the latitude and longitude values for records, as that will typically be handled automatically if you have enabled address geocoding for your site.

We can see the **Address** pane in the following screenshot:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_160_0.png)

### Communication preferences

Now that you have your contact record in place and have recorded all the various ways you might be communicating with them, CiviCRM provides tools for further classifying (and limiting) your communication methods. These preferences are broken into four areas:

- Greeting preferences are used when e-mailing, generating labels, or exporting contacts, and can use fields from the record (tokens) or be fully customized.
- **Privacy settings** reveal if the person prefers not to be contacted through a certain method. The **Privacy** settings do not lock access to the communication details; they are simply a tool allowing CiviCRM users to quickly see that the contact has requested not to be contacted in the selected way. If selected, the privacy settings generate an eye-catching "stop" icon next to the respective method in the **Contact Summary** tab.
- Similarly, the **Preferred Method(s)** are used for your internal purposes in order to facilitate and improve communication with contacts.
