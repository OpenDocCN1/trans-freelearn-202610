
## Managing Events

The import tool is very similar to what we saw in other areas. The four-step wizard consists of loading a file and configuring the import settings, mapping the file's fields to CiviCRM fields, previewing the results, and completing the import. An error file will be generated and will be available for download if any problems are discovered with any records.

To access the import tool, visit **Events | Import Participants**. There are a few things to note that are specific to importing events:

- Participant import only accepts `.csv` files. You cannot connect to a MySQL database as with the contact import.
- The most significant difference between importing participants and importing contacts is the behavior of the **Update** option for handling duplicates. The Update option *requires* the presence of an existing participant record, which *must* be identified using the unique participant ID value. Consequently, you will only use it in scenarios similar to the one we just discussed, where the participant list is exported from the system, changes are made, and it is then imported back into the system. If the **Update** option is used, CiviCRM will not process new registration records. In this way, it differs from the contact import that matches and updates existing records *and* creates new records for those that do not match.
- As one might expect, the field mapping options available for the participant import include a number of registration-related fields. Take note of those in red, as they are required in order to successfully import these records. They include the Event ID and Participant Status. The former can be obtained from the **Manage Events** page. Several of the fields highlighted in red are used for matching to the contact record. Not all of these are required; only ones sufficient for making a valid match are required. For example, you do not need the internal contact ID and the first/last name. Either of these is sufficient for making a match.

## Tracking, searching, and reporting

In the last section, we worked with an individual contact record to see how the registration is created with an associated contribution record. In this section, we will begin working with multiple participant records.

## Tracking registrations using the dashboard

One of the most important things for you as an event manager is the ability to quickly see how many people have registered for a given event. CiviCRM provides you with a snapshot to these details on the **Events | Dashboard** page.

Similar to other areas of CiviCRM, the **Dashboard** provides a snapshot into key data in a summarized fashion. For events, this consists of a listing of upcoming events with registrant counts broken down into various categories, and a list of recent registrations received.

The registrant counts are very useful and bear some explanation. Review the following screenshot:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_387_0.png)

CiviCRM helps you *understand* your participant records by breaking them down into categories. We begin with the number of **Counted** registrants, in contrast to those **Not Counted**. Counted participants include those who have registered, including both those who have paid and those who will pay later. **Not Counted** is broken up by status and role. By default, registrants with a status of **no-show**, **cancelled**, **pending from incomplete transaction**, **on waitlist**, **rejected**, and **expired**, will all be excluded from the registrant count. Why? This is because they are not expected to attend or are confirmed to not have attended. In a similar way, participant roles may be configured to be included or excluded from the total counts. In the example we saw earlier, we had created a participant role entitled "Staff" which should not be included in the count.

Settings for the participant statuses and roles can be configured through **Administer | CiviEvent**, and were discussed earlier in this chapter. What we see in this section is the impact of those settings and how they are used to help you understand the nature of your participants.

The **Registered**, **Pending from Pay Later**, and **Cancelled** counts you see in the screenshot reflect the various participant statuses represented by the records. If other statuses were present, they would be listed as well.

Note that each of these counts is a live hyperlink; selecting the link displays the relevant registrants in that count. This is, by far, the fastest way to view and work with a segment of your participant records.

## Managing Events

## Searching for participants

While the dashboard provides a quick window into a single event, you will undoubtedly want to search for records based on more refined criteria or across multiple events. We do this through **Events | Find Participants**, as shown in the following screenshot:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_388_0.png)

The fields available through this tool are fairly self-explanatory. You have options to search within a single event, across multiple events within an event type category, by date ranges, or fee ranges. The search fields are restrictive; all criteria must be matched in order to return results. Entering a large number of criteria will result in a smaller number of records returned. The two exceptions are the multi-select fields, **Participant Status** and **Participant Role**. Multiple selections within that field are inclusive, though its interaction with other fields remains restrictive.

Similar to CiviCRM's other search tools, you may take action on bulk records after running a search. First select all records or choose selected records and then initiate your action from the drop-down menu.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_388_1.png)

*Managing Events*

- Event Income Report (Detail)
- Attendee List

The **Event Participant Report** and **Attendee List** are both based on the same template and basically provide options for retrieving registrants based on the role, status, event(s), event type, or date range. It is great for generating a list of attendees for staff to keep on hand during the event, or to publish an attendee list for distribution to attendees. One of the unique things about this report is that you can include blank columns before or after the data columns. This could be used to track attendance by checking off registrants on the list as they check in to the event or to collect other information to be imported later.

The **Event Income Report (Summary)** displays event income details and breaks out the participant status categories in the event. Using this report, you gain a sense of how your registrants versus cancelled or other non-counted statuses may have impacted your net income.

**Event Income Report (Detail)** generates a detailed breakdown of all major categories of the event, including statuses and roles, and generates counts and income totals for each line. It provides an excellent "final report" tool for your event file.

## Integrating events into your CRM strategy

One of the challenges of writing a book about software like CiviCRM is that it's really easy to remain modularized—and consequently, it would be easy for you to approach the software in a purely modularized way. That's fine to a large extent; you will naturally think and be structured around these blocks of functionality such as events, members, donations, case management, and so on. However, every so often, it's good to think outside of those modularized "boxes" and push beyond to realize the full potential of the software.

You've created an event, published the registration form, collected registrations, pulled your attendance list, printed name badges, and presented a report to your Board of Directors. In short, you've experienced a successful event management process.

But what have you done to invite attendees to take further action? Have you encouraged them to become involved in a committee, sign up for an e-newsletter, or join the organization as a member?
