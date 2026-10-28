
Contact subtypes are created and managed through: **Administer | Option Lists | Contact Types**. From this screen, as seen in the following screenshot, you can rename the existing types (including the three primary types), create new types, and assign icons to each type in order to help visually distinguish them while reviewing the records.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_152_0.png)

## Planning your contact types

It is important to give a careful thought as to how you work with different types of contacts when you configure and build out your CiviCRM implementation. Contact types may be viewed as the highest level of categorization in your system and the choices you make will have implications throughout the rest of the system.

Consider the following as you plan your contact types:

- Don't overuse contact subtypes. There will be a plenty of other ways to categorize and segment your records. The subtypes are best used when you have distinct differences between contacts that strike at the core nature and role of the entity.
- Contacts can be assigned only a single contact type; don't create a structure where there may be confusion or overlap between subtypes. In the food pantry example, you would not want to create subtypes for food supplier, grocery store, and convenient store because the grocery store and convenient store are both food suppliers, and having those three subtypes will create uncertainty about where a certain business best fits. It would be better to create a single subtype for food supplier and use other means (such as tags or a custom field) to further categorize within that one type of entity.
- If you are planning to migrate data from an existing database, be sure you take the time to organize your existing records into the types and subtypes you plan to use in CiviCRM. The contact import tools provided by CiviCRM require you to import a single contact type at a time. You will want to make sure you've settled on your contact structure and prepared your import data accordingly before you begin importing.
- One of the primary benefits of contact subtypes is the ability to create sets of custom data fields for one or more types. The custom data fields are those fields which you create to extend the core fields supplied by CiviCRM out of the box. This is a useful way to think through the planning process: Do I plan to collect data for this one type of organization that I don't need or clearly does not apply to another type? If so, it may warrant creating a subtype.

You assign contact subtypes when you create contact records. If an existing contact does not have a subtype selected, you may specify one when editing. However, if you've selected a subtype and stored any custom data associated with that type, you will no longer be able to change the subtype (unless all custom field sets for that subtype have been permanently deleted). For example, FPAGM creates a custom data field to track the daily average of the number of clients a food pantry services. This field is attached to the Food Pantry subtype, as it is only relevant to those contacts. If we create a new organization record, assign the Food Pantry subtype to it, and save the organization contact, the record is now locked on that subtype; we cannot decide to change the record to the Food Supplier subtype at a later date. This system behavior ensures protection against unintended data loss or orphaned data. If at a later time you decide to completely delete the custom data fields associated with the subtype, you will once again be able to reassign a contact to a different subtype. Basically, CiviCRM locks the subtype in order to prevent data loss — if there is no subtype-specific data stored for a record, you can change the value without risk.

While we are discussing contact types, there is another important consideration to think through, particularly if you have a complex organization with multiple administrative users who will be working with CiviCRM.

As we will see shortly, a large part of working with CiviCRM involves connecting related records with the contact record. For example, you may track phone calls and meetings held with an individual, donations contributed by a business, membership from a household, or open a case for any of these contacts. Very often, there are connections built between related contacts and in each instance, you must decide where the contribution, membership, or other records should be attached.

John Smith, the proprietor of ACME Convenient Store, calls FPAGM to arrange the donation of 50 lbs. of bread for distribution to food pantries. FPAGM staff will track this as an in-kind donation in CiviCRM. The question asked is should the contribution be recorded with John Smith (after all, he is the owner and primary contact), or with ACME Convenient Store?

In most cases, it is very clear where the associated record should be attached. However, where there is potential for confusion and ambiguity, you will want to establish clear policies and procedures to help staff determine the proper location for the data.

Consider the following questions:

- Where is the official "source" of the data? If you receive similar data in the future, where will it be tracked? In the preceding example, we would record the donation with the organization (ACME Convenient Store), not with John Smith. It is really the business that is contributing, not the individual.
- Who is actually involved in the activity? For example, who will be attending the event, or whom will the official tax receipt be made out to? One common area of confusion is an organization that hosts trade shows as part of a conference. Are your exhibitors the businesses themselves or the individuals representing the business? This can be tricky, because "businesses" don't attend events, people do, and yet a good argument could be made for recording the exhibit-related details with the organization record.

Regardless of how you resolve those potential areas of confusion, it is important to strive for consistency. How you enter your data will have a direct impact on what you are able to pull from the system.

Once you've defined your contact subtypes (if any) and thought through the policies you will put in place for the staff, take the time to manually create a few records with real data to test your structure and policies. This may help you identify and correct any unanticipated consequences of your configurations.

## Core information fields

Let's get into a contact record and begin to see how this idea of a contact with related records takes shape in CiviCRM. If this is your first time in CiviCRM, place your cursor in the search box to the left of the CiviCRM menu bar, as seen in the following screenshot, and then click on Enter to view all the records:

At a minimum, you should see one record corresponding to your website user account (you may also have seen that record in the left sidebar recent items list). Enter this record, or any other one, to view the contact details.

As you can see, the contact record is displayed through a series of tabs corresponding to the different types of data you may be collecting.

> The tabs displayed in a contact record depend on configuration settings in two places: **Enable Components** (Administer | Configure | Global Settings | Enable CiviCRM Components) and Site Preferences (Administer | Configure | Global Settings | Site Preferences). If you do not see an expected tab, make sure that the component is enabled, and that you have indicated that it should be viewable on the contact record page.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_155_0.png)

We will not discuss all of the available tabs at this point, but only cover those that are directly related to the contact record itself, including communication tracking, relationships/connections with other records, and core categorization tools.
