
## Why host events?

Before digging further into things, it's worth taking a minute to ask the questions: why would you host events, and what exactly is an "event" as it relates to CiviCRM?

Many not-for-profits exist as organizations that people donate to, become members of, or support in other ways. The purpose and services many organizations provide are not oriented around "walk-in" support or other face-to-face interactions with constituents, apart from events.

Events provide an important (and often, the only) in-person interaction with your supporters, members, and others committed to the mission and vision of your organization. As such, the face and voice of the organization is most clearly seen and realized during events.

So what exactly is an "event"? In CiviCRM, event management tools allow you to do the following:

- Publish and advertise an event description including date, time, and location
- Register participants
- Calculate and collect fees
- Collect data about participants
- Track registrant's status
- Use more advanced elements, such as self-registration forms on your site with automated waiting lists

You are not likely to use the event tools for very small group meetings, such as a one-to-one meeting with a constituent, board meetings, committee meetings, and other such no-fee, basic events. For these events, it will be easier and simpler to use a CiviCRM activity, either with the pre-configured meeting activity type, or through your own custom activity types.

## Building and promoting your event

The first step in event management is to configure your event in the system. Once configured, you can begin collecting registrations and tracking participants. The event tools all reside under the navigation bar's Events menu. As with other areas of CiviCRM, the event administration tools are not always provided in a simple, task-oriented workflow. You are likely to find that the creation, configuration, registration, tracking, reporting, and managing happen in a more iterative pattern, rather than a linear one. We will begin by walking through the event creation process, touching on the various options available, and may circle back later to delve deeper into various areas.

Use the **New Event** option to begin the process of setting up an event. This opens up an initial information and basic settings form. After saving it, you will be directed to a tabbed interface where you optionally configure four other settings areas (or return to adjust this first form).

You should carefully work through each tab in a sequential wizard-style way when first becoming familiar with the event tools.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_363_0.png)

## Information and settings

The first form defines basic information about the event, including its category (type), title, description, dates, and so on. You may notice that the very first field on this form is for selecting an event template. If no event template exists, a notice to this effect will appear at the top of the page.

Event templates are very useful if your organization hosts a number of very similar events, such as a monthly breakfast seminar series, or a bi-monthly training workshop. From one event to the next, the structure, fees, location, and perhaps even the basic description remains similar. In such cases, you can save time and reduce errors by setting up an event template under Events | Event Templates. The template creation tool is almost identical to the five-tabbed form we will be working through now, with the exception of a few fields removed (such as event dates) that are presumed to be unique for each event.

When creating a template, complete only those fields which are common to all (or most) events of this type. For example, if your monthly breakfast seminar hosts different speakers on different topics, you will want to leave the summary and description fields empty.

When a previously configured template is selected while on the **Info and Settings** form for a new event, the event is pre-populated with any data stored in the template. You will have the opportunity to review, adjust, and fill in any fields in your event. Once a template has been selected for a new event, there is no association back to the template. In other words, changing template settings at a later date will have no impact on existing events that used the template. Its only purpose is to pre-populate the event, and in doing so, to facilitate the event creation process.

## Managing Events

Returning to the initial event creation form, we see event type and participant role fields. The event type is used to categorize your events, which can be useful when analyzing and managing events. You can, for example, define an event type for your annual fall conference and later run searches based on the event type, such as displaying all constituents who participated in any of the fall conferences over the last five years. The event type options are managed under **Administer | CiviEvent | Event Types.**

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_364_0.png)

> > While participant listing pages may serve as an effective peer-pressure promotion tool, they may also be perceived as invasive to privacy by your attendees. Make sure the nature of the event (and the nature of your constituents) supports displaying such a list. In particular, be sensitive to the use of the name and e-mail template, as some contacts may not want their e-mail information disseminated in this way.

Enter the title of your event, keeping in mind that it will be publicly visible in the event information and registration pages. You also will want the event to be uniquely named to avoid confusion with other events. For example, if you host an annual conference, you might name it: Annual Conference 2010, Annual Conference 2011, and so on.

Use the **Event Summary** and **Complete Description** fields to describe the event, such as the topics covered, speakers, audience, and so on. As the name suggests, the **Event Summary** field should be brief and succinct. It will be included in RSS and iCal feeds generated by the system, which you may use for promotional efforts. The **Complete Description** field should be more complete, and is displayed on the event information page.

> > Your RSS feeds allow other websites to automatically pull content from your site and display it on their site. The event feeds include information about your events, such as links to send their visitors to your registration pages. iCal feeds are another similar format for publishing your event information. People can set up their Google Calendar, Outlook, and other similar calendaring applications automatically to read in these feeds and display your events on the right date and time.

Define the event's start and end date/time. For a single-day event, it is sufficient to just complete the start date field.

We'll spend a bit of time in a moment on the waitlist option. Proceed to the final set of fields on this first step of the event wizard, and click to enable or disable the following options:

- **Include Map to Event Location?** Inserts a Google or Yahoo! map on the information page with the event location plotted. You must have a location defined on the second step of the wizard and must have geocoding configured in your **Global Settings** to make use of this function.
- **Public Event?** Determines if the event appears in RSS/iCal feeds, in the HTML event listing page, and in Drupal's CiviCRM **Upcoming Events** block. Turn this off if you have "invitation-only" events that you don't want to publicize through the automated methods.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_366_0.png)

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_366_1.png)

## Managing Events

In either case, registrants remain on the waitlist until space becomes available, at which point they are sent an e-mail inviting them to register. We will see one additional option impacting this process on the **Online Registration** form — the ability to define an expiration period for their **Pending** status. Let's review how the process works through an example:

- 1. While attempting to register, an individual is informed that the maximum number of registrants has been met.
- 2. The person chooses to go on the waitlist, and then enters his/her name and e-mail address.
- 3. After three days, your organization decides to move the event to a new venue that accommodates 10 more people. After altering the settings in the system, the first 10 people on the waitlist (first come first served) receive an e-mail, inviting them to register. CiviCRM automatically changes the status of their registration from **On waitlist** to **Pending from waitlist**. If they have not registered after the expiration period is over, then their registration status changes to **Expired**.
- 4. During the period when the status of these 10 registrations is **Pending from waitlist**, only those waitlisted contacts may register. New visitors to your website will not be able to register, as preference is given to waitlisted people. Waitlisted individuals beyond the first 10 (who were invited to register) remain on the list. As these waitlisted individuals respond to the invitation to register or allow the invitation window to expire, new people are invited to register (if there are additional individuals on the waitlist) or the registration form becomes publicly available.

While the assumption is that those 10 invited people on the waitlist will sign-up in a timely fashion, that's not always the reality of the situation. Some of those people may drag their feet or simply lose interest in the event. In either case, your ability to successfully fill the event to capacity and give fair opportunities to others on the waitlist to attend is compromised.

The expiration period option improves this by limiting the window of time during which places are held for waitlisted invitees to register for the event. We will review how to configure it in the **Online Registration** section. There are a few things to consider when using this feature:
