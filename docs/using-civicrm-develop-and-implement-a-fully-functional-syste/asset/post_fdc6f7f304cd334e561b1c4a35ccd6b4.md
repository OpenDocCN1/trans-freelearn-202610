
- **Record Activity for Contacts:** Creates an activity record and associates it with multiple contacts at once. One helpful use of this option is to record mailings. Let's say you run your search and want to export the records for delivery to a mail house. After running the export, add an activity to the same result list to record the details of the mailing, for example, "Volunteer Solicitation Letter", with an attached copy of the document used for mailing.
- **Add Contacts to Event:** Registers the selected contacts for an event. The only limitation of this action is that you have to select a single set of registration options to apply to all contacts selected. You are also not able to record payment details in bulk.

## Update contact actions

Several actions operate directly on the contact records, allowing you to merge, update, delete, or undelete them:

- **Batch Update via Profile:** Profiles are collections of fields that you've defined in the system. You may use profiles to edit up to 100 contact records simultaneously.
- **Merge Contacts:** Trigger the deduplication / merge tool using the selected records. You will be redirected to a page where you can review the records and choose the fields that will be merged from one record to the other.
- **Delete Contacts:** Move the contacts to the trash (if this system is enabled).
- **Delete Permanently:** Completely remove the contacts from the system.
- **Restore Contacts:** If you've searched for deleted (trashed) contacts, you may use this option to restore them in bulk.
- **Unhold Emails:** Remove the **On hold** e-mail status for selected records.

## Export/map actions

The following two actions support doing something with contact records, rather than changing them:

- **Map Contacts:** If geocoding is enabled on your site, you may create a map with the selected records pin-pointed on it.

## CiviCRM Basics: Moving through the System and Working with Contacts

- **Export Contacts**: Inevitably, you will need to conduct searches and export the results to a `.csv` file (comma-separated values) to be used for various purposes—sending to a mail house, conducting calculations on the data, merging letters, transferring to someone via e-mail, and so on. The export records process consists of two steps:
  - Choose the export type—default primary fields or your own selection of fields. You may save export mappings (collections of fields) for reuse at a later time.
  - If you've chosen to select your own fields, you will be shown a grid where you choose the contact type, field, and relevant field parameters. At the bottom of the page, you can save the export mapping, update an existing one, and export the list to `.csv`. When defining your field selections, remember that the field will only export data if it is appropriate to the contact types in your search results. For example, if your search results contain only individuals, any household fields defined in your export will be empty. Also note that there are options to export related records. For example, if I would like to retrieve the State value for the organizations associated with people through the employer relationship, I can do so using the "related contact info" options toward the bottom of the field list.
  - When exporting records, you may merge records with the same address or merge household records. The former option will identify contacts with the same address and merge them into a single record. The latter option will look for records in your result list that are part of a household, and export the household record instead of the individual contact records. Both of these options will help prevent sending multiple mailing pieces to the same address, reducing your mailing costs. Note that these options do not impact the records themselves; they only impact how the export file is generated.

## Subsequent actions on the same selection

You will find in many cases that after triggering an action, you can return to the search result list and trigger a follow-up action. The record activity example above is a good example of where this can be helpful—after exporting your records for a mailing, perform a second activity action to record the mailing details.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_189_0.png)

> FPAGM has an upcoming Board Meeting and must notify the Board members about the logistical details. They use the simple search to retrieve all contacts which are a part of the existing Board of Directors static group. From the action list, they choose to send an e-mail to all the records in the result list. The e-mail contains information about the location, time, parking details, and a copy of the agenda as an attachment. In addition to the direct benefit of reminding the Board about the meeting, FPAGM values the fact that a record of the e-mail is attached to each contact as a completed activity. This provides a historical record of communication with the Board, and may be helpful should one of the Board members claim that they did not receive the reminder.

## Working with contact records

Throughout this chapter, we've covered a lot of ground with regard to contacts. However, our emphasis has been on understanding the types of data collected, navigating through the system, and working with search tools and results. Let's wrap this chapter up by looking at alternate workflows and record maintenance through deduping tools.

## Alternate workflows

We are working within a CRM, a Constituent, or Contact relationship management tool. So it's natural to view your data through the lens of contacts. Just about everything we've covered thus far has done this; we've either been working inside a single contact record or we've conducted a search to take action on a group of contact records.

However, sometimes it may seem more natural to start with the action instead of the contact record. CiviCRM provides several options for this type of alternate workflow.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_190_0.png)

On the left sidebar, you will see a **Create New** button and a drop-down menu. This button is always visible in Joomla!; and is a **default-enabled** block in Drupal. Drupal users may configure it through the Drupal **Administer | Site building | Blocks** tool. In addition to options for creating contacts, there are options available for creating an activity, contribution, e-mail, event registration, membership, pledge, group, or tag.

Many of these options are also listed under the appropriate main menu item (for example, **New Contribution** is listed under the **Contributions** menu).

Choices that will create records attached to a contact will have a contact field on the form where you can begin typing an existing contact name or choose to create a new contact from the drop-down.

Creating a new record "on-the-fly" causes a pop up window to appear with a few contact fields. This form is configurable through **Administer | Customize | CiviCRM Profile**. By default, it contains only a few required fields. You may want to expand what fields are included in order to ensure that the system users collect more information when they are entering records into the system.

The profiles used for "on-the-fly" contact creation are entitled **New Household**, **New Individual**, and **New Organization**. Notice in the CiviCRM profile listing page, that these are marked as **Reserved**. Throughout CiviCRM, any records marked as **Reserved** cannot be deleted, and will be used for special purposes in the system. It is recommended that you do not make any radical modifications to reserved records, as it may have unintended consequences in the system. For example, we advise against renaming the **New Individual** profile, and then using it as a contact signup form.

## Eliminating duplicates

Data is king. Even the most advanced CRM tools will be virtually useless if the data it is handling is incorrect, messy, or outdated. Your system will live or die based on the principle of garbage in/garbage out.

What steps can you take to improve the quality and reliability of your data? One important way is to reduce record duplication.

Record duplication is an unavoidable reality, especially once you begin to expose forms to the front of your website and invite constituents and site visitors to submit their own data. Fortunately, CiviCRM has several ways of dealing with record duplication.

## Strategies for dealing with duplicates

There are three strategies we will use in the fight against duplicates:

- **Policies and procedures:** Yes, the first tactic is a human resource and management strategy. Anyone who is working in CiviCRM as an administrator must understand the importance of reducing duplication. You can start this is by enforcing a policy where every time a user is going to work on a contact record, they begin by doing a search for the contact. Simple, right? How many times do people enter the system and immediately begin by creating a new record? CiviCRM's auto-fill search tools will help significantly, but they will only benefit you if they are used.
- **Inline deduping:** While creating or editing a contact record, CiviCRM does a lookup to see if there is a potential match with an existing record. If so, you are given the option of visiting the potential duplicate record or continuing to save your current record. CiviCRM uses deduping rules (discussed in the following point) to define what potentially constitutes a match. The default fuzzy rules are used for inline deduping. You may also trigger a dedupe check with the **Check for matching contacts** button.
