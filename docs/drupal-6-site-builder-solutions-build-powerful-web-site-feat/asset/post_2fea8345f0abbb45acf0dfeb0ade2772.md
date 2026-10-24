
## Newsletters and Calendars

It is highly recommended that you send a test newsletter before you send it to the entire subscription list to prevent errors in the final newsletter. The test address defaults to the administration email address for your site. You can change the test address on the Simplenews Settings page, which is accessed by selecting Site configuration and then Simplenews, from the Administer menu. The available settings are shown in the following screenshot:

Test addresses
Supply a comma-separated list of email addresses to be used as test addresses. The override function allows to override these addresses in the newsletter editing form.
Email address:
admin@drupalbyexample.com
Allow test address override

- 4. When you are satisfied with your issue, click the Save button.
- 5. After you have sent a test email and checked that it was delivered correctly, you can send the newsletter to the full list of subscribers by editing your draft and then changing the Newsletter sending options to Send newsletter.
- 6. Once the newsletter has been scheduled to be sent, it will be sent to a portion of the subscriber list each time that cron is run. This is done to avoid flooding the network. You can configure the number of emails that are sent each time that cron is called by editing the Mail backend settings for the Newsletter, which is accessed by selecting Site Configuration and then Simplenews from the Administer menu. The available settings are shown in the following screenshot:

Mail backend options
Use cron to send newsletters
When checked cron will be used to send newsletters (recommended). Test newsletters and confirmation emails will be sent immediately. Leave unchecked for testing purposes.
Cron throttle:
20
Sets the number of newsletters sent per cron run. Failure to send will also be counted.
Mail spool expiration:
Immediate
Newsletter mails are spooled. How long must messages be retained in the spool after successful sending. Keeping the message in the spool allows mail statistics (which is not yet implemented). If cron is not used, immediate expiration is advised.
Log emails
When checked all outgoing simplenews emails are logged in the system log. A logged email does not guarantee that it is sent or will be delivered. It only indicates that a message is sent to the PHP mail() function. No status information is available of delivery by the PHP mail() function.

By default, only 20 emails are sent each time that cron is run. This is fine, if you are running cron frequently, but if you are running it less frequently, you may want to increase the number of emails that are sent each time. Some web hosting companies only allow a specific number of emails to be sent every hour, to help prevent spammers from utilizing their servers. Before sending newsletters to large mailing lists, you should check with your host to ensure that you comply with their policies.

### Adding a calendar

A calendar is a fantastic way of keeping customers coming back to your site and your business at regular intervals so that they can take advantage of specials, sales, and other time-limited events.

We will create a basic event calendar, which displays information about musical concerts, special events, and more, for the Good Eatin' restaurant.

### Adding new events to the calendar

Good Eatin' Goal: Create an event that will be displayed on the calendar.

Additional modules needed: Event (http://drupal.org/project/event).

#### Basic steps

- 1. In order to add an event, you must first install and activate the Event module in the Module manager as shown in the following screenshot:

   ![](img/b332d58f89551dfc3ae9a7cf47e6a08f_198_0.png)

   Activating the Event module will create a new Event content type. There are also several settings that control how events are displayed and how time zones are handled.

- 2. To modify the time zone settings, select Site configuration, then Events, and finally Timezone handling, from the Administer menu.

- 3. Drupal will display a page similar to the following:

   ![](img/b332d58f89551dfc3ae9a7cf47e6a08f_199_0.png)

- 4. You will want to customize these settings according to where the majority of your users live, and the types of events that you are holding. For example, if most of your users are in the U.S., 12 hour notation is probably appropriate, but if most of your users are in Europe, 24 hour notation is better. If the events are held online with a mix of users in different time zones, it would make sense to have the events displayed in the user's time zone. However, if the event is being held at a single site, it would make sense to use the local time of the event. For the Good Eatin' site, we will use the site's time zone for events, display the events in the event time zone, and use the 12 hour time notation.

- 8. Enter a title and a description for the event, as shown in the following screenshot, and then set the start time and optionally the end time for the event.

   Create Event

   Title: *
   Winter 2008 Menu party

   Menu settings

   Body:
   Join us for the unveiling of our Winter 2008 seasonal menu. You can sample all dishes from the new menu and enjoy complimentary beverages designed to enhance the flavors of the menu.

   Split summary at cursor

   Input format

   CAPTCHA

   ☑ Event has time
   Is time important for this event? Uncheck if event takes all day.

   Start date:
   September 01 2008 18 00

   ☑ Event has end date
   Check if you want to specify an end date for this event, then choose end date below.

   End date:
   September 01 2008 22 00

- 9. Click Save when you are happy with the event settings.

### Displaying events

Good Eatin' Goal: Display events on the site in various formats including a block of upcoming events, a table of events, and a calendar of events.

Additional modules needed: Event (http://drupal.org/project/event).

#### Basic steps

The Event module provides several methods for allowing customers to view events. We will explore each of these in turn.

- 1. The easiest way to allow visitors to browse events is by using the event page, which is accessed at http://yoursite.com/event. The page appears as follows:

   ![](img/b332d58f89551dfc3ae9a7cf47e6a08f_202_0.png)

- 2. If you want the user to be able to access this page without knowing the URL in advance, you can create a menu item for the page. Open the Menu Manager by selecting Site building and then menus, from the Administer menu.

- 3. Select the menu that you want to add to the menu item and then click the Add item tab.

- 7. The new block will appear as follows:

   ![](img/b332d58f89551dfc3ae9a7cf47e6a08f_204_0.png)

- 8. The final method of displaying calendar entries is a block showing upcoming events in a calendar view. To add this block, open the **Block Manager** by selecting **Site building** and then **Blocks**, from the **Administer** menu. Set the region for **Calendar to browse events** to **Right sidebar**.

   ![](img/b332d58f89551dfc3ae9a7cf47e6a08f_204_1.png)

- 9. The display for the calendar will appear as follows:

| Sun | Mon | Tue | Wed | Thu | Fri | Sat |
|---|---|---|---|---|---|---|
|   |   |   |   |   | 1 | 2 |
| 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| 10 | 11 | 12 | 13 | 14 | 15 | 16 |
| 17 | 18 | 19 | 20 | 21 | 22 | 23 |
| 24 | 25 | 26 | 27 | 28 | 29 | 30 |
| 31 |   |   |   |   |   |   |

You can decide which of these methods to use for your own site, based on how the user will work with your site.

### Adding other content types to the event calendar

Good Eatin' Goal: Discuss how to add custom content types to the event calendar.

Additional modules needed: Event (http://drupal.org/project/event).

An easy way of adding additional content types to your existing event calendar is by modifying the content type and then setting the Event calendar options.

- 1. Open the Content Manager by selecting Content management and then Content types, from the Administer menu.
- 2. Edit the type that you want to add to the event calendar.
- 3. Open the Event calendar section and modify the options, as shown here:

   ![](img/b332d58f89551dfc3ae9a7cf47e6a08f_205_0.png)

   If you prefer to have a calendar just for the type, you can use the Only in views for this type option.

- 4. Save the changes to your content type, and the event calendar will be automatically updated.

### Creating events using CCK

Good Eatin' Goal: Build events using the CCK module and the Date module, rather than the Event module, thereby giving additional control over the events.

Additional modules needed: CCK (http://drupal.org/project/cck), Date (http://drupal.org/project/date).
