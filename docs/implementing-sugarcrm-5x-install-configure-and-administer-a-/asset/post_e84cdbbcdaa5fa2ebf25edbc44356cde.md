

Now that the record has been saved, the detail view for the **RayDoc** account should contain the **Doc Newhart** entry within the Contacts subpanel, as demonstrated in the following screenshot:

![](img/7ccaf60360d7c8e809d336edbba2263d_109_0.png)

To reinforce the ability to link back and forth, up and down, and sideways within SugarCRM (and any good CRM system)—click on the contact name of **Doc Newhart** to view his Contact Detail view screen. From that screen, click on the account name link to return to the Account Detail view screen for RayDoc. Notice that the **Last Viewed** links area on the upper left, just below the navigation tabs, is accumulating a list of recently accessed records to simplify the process of returning to them.

Now add another contact to the RayDoc account entry, repeating the process that we just described. However, do not enter any other data than your name. You should now see two contacts listed in the **Contacts** subpanel of the RayDoc account detail view screen. This is another important point. We have re-used all the basic account information in the main panel of the RayDoc account for the second contact. We did not have to type it in again. In addition, if any of the RayDoc account information changes, we will only have to edit the RayDoc account record, not the information that we have about our contacts there. This is one of the key advantages of a CRM system over a contact manager like Microsoft Outlook 2003. It understands that accounts and contacts are two separate types of information that are linked together, not crammed together into one unsatisfactory amalgam.

## Data relationships and searching

Again, to reinforce the speed and power of navigation in SugarCRM, browse to your own contact record. Now go back to the RayDoc account detail view and then on to Doc’s contact record. A contact can lead to an account, which can lead to other contacts at that account—all rather quickly. Moreover, it is very relevant to your everyday work needs.

Go ahead now and explore the contact list view on your own by hovering over the **Marketing** tab and clicking on **Contacts**. Notice that you can search for a specific contact in the **Contact Search** panel above the list of contacts. Clicking on **Advanced Search** displays a more comprehensive list of fields to search, as displayed in the following image:

![](img/7ccaf60360d7c8e809d336edbba2263d_110_0.png)

One helpful feature inherent to the search capabilities is the idea of being able to search for an individual based on their first or last name independently. As your database grows, the value of this feature will become more apparent as there will be many cases where you might not remember an individual's full name, but do remember their first or last name.

Along these same lines, it allows you to search for portions of a value by means of a wildcard character. For example, to search for contacts whose last name contains the letters son, as in Gunderson, you could type %son in the Last Name field and SugarCRM would yield the matching results.

## SugarCRM basics: Security

Now that you have some familiarity with the process of entering data, searching, and establishing relationships among different entries, it is time to discuss the important topic of security and access control. Other than the earlier mentioned username and password, you may have a need to control the records that are accessible to the various users who will be accessing your SugarCRM installation.

This is an important topic to discuss with your stakeholders and project leaders. By its very nature, CRM systems are designed to allow all users access to all data by default. This makes perfect sense, as the core philosophy of the system is to facilitate communication among a group of users. Over time, however, you will want to make adjustments to the security settings to control which users can view, edit, or delete certain data.

For now, the most obvious component of security that you will note is the use of an **Assigned to** field on all data entries. As you work with SugarCRM, you will notice that every record in the database, regardless of whether it is an Account, Contact, Opportunity, or other, contains this field. By default, SugarCRM will automatically populate this field with the name of the user that initially entered the record into the system, but allows users to select a different assignee.

Note that by default, it is only possible to assign a record to a single user. The ability to assign data to multiple users, referred to as a **Team**, is out-of-box functionality that is meant only for Professional and Enterprise Edition subscribers.

This mechanism is integral to the security underpinnings of SugarCRM. While it may be of little significance if you are a single user, tampering with the field can cause problems, such as not being able to access certain records and other oddities.

More advanced security features, discussed in _Chapter 5_ of this book, all rely on this field, hence it is important.

## The sales pipeline: Leads and opportunities

Now that you have practiced putting accounts and contacts into the system and linked contacts to accounts, let us go on to learn about a few more data types your CRM system can organize for you.

Most modern CRM systems are designed to incorporate the concept of _leads_ and _opportunities_ as independent data entities. They are kept as separate items of information, even though they may sound very similar to each other. The generally accepted distinction between them is that a lead is information pertaining to an individual that has come to you in such a way where you cannot be certain that they are interested in your products and services. They need to be qualified. On the other hand, an opportunity is what a lead becomes once you have been in touch with the person and confirmed that they do have a genuine and qualified interest in what your company has to offer (they have a real business problem, the money to fix it and you’re talking to someone who has purchasing power).

In CRM terms, the process of selling goods and services to your customers comes down to a series of steps that look something like the following:

- Acquiring and tracking leads
- Qualifying them as real opportunities
- Quantifying these opportunities and setting an expected closing date for them
- Aggregating all opportunities together into a sales pipeline to see the big picture
- Closing the opportunities as sales

The intended benefits to your business from such a design are as follows:

- You can forecast your expected sales over the next couple of months in a more tangible and quantifiable way than ever before. The sales pipeline is your distant early warning system, as it shows you when your business levels are falling off, or growing faster than anticipated. In a smaller business, it is your best friend when it comes to business planning, giving you a more detailed and distant view into the future, helping you to make appropriate staffing and expenditure decisions.
- All your sales leads, opportunities, and account history are now formally recorded in a central system that the business owns. Your vulnerability to threats and blackmail from sales personnel, or to the sudden defection of sales personnel, is reduced. If a sales person leaves the organization, you still have all the leads and opportunities the business paid to develop, along with the account history needed to help a new sales person take over the account quickly and effectively.
- The sales person on an account is not the only person in the organization with access to details of activity history. Other personnel with direct customer contact now have information that empowers them to make more educated decisions about issues relating to any of your customers.

How did all of this impact Doc after the initial roll out of the system? Our initial task was to find all his leads. Most of them were fairly dormant in the inboxes of the sales staff—something to do on a slow day if they felt like it.

So Doc had to round up all the sales people and harvest all these leads. On an ongoing basis, most leads came to the company, from suppliers that RayDoc partners with, and not directly to the individual sales people, and so could be entered in the system by Kay, in her spare time. They would then be simultaneously assigned to a sales person, ensuring someone would be responsible for tracking them. Leads that came directly to a sales person would have to be entered by that sales person—and this would be used as matter for training in the near future.

