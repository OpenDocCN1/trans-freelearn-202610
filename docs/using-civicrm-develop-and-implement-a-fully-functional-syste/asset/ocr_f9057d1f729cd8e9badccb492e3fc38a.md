+   - **Registration (live)**: Displays the live registration form:

![](bbox=[0.373, 0.175, 0.627, 0.27])

> Although CiviCRM does provide a test-drive option, we recommend testing with the live registration link until you are comfortable with the configuration options. You may want to temporarily add a small-value registration fee option for live testing with a credit card in order to avoid the cost of issuing a refund.

The live links above (**Event Info** and **Registration (live)**) can be used in your website content, e-mails, and other promotional methods. Joomla! users may also construct links to these pages through the creation of a CiviCRM menu item.

In addition to direct links to your event pages, you may want to provide access to lists of upcoming events. There are several ways to do that using native CiviCRM methods. From the **Manage Events** page, you will find four icons in the top-right region of the **Find Events** block:

+   - HTML listing of events
+   - RSS feed of events
+   - iCalendar file download of events
+   - iCalendar feed of events

Each link publishes a list of current and future events that are designated as public. Recall that in the first page of the event setup process, we had the option of setting an event as public; the primary use of that function is to control what appears in these listing pages.

![](bbox=[0.415, 0.678, 0.585, 0.715])

CiviCRM provides Drupal with a block to display events. You can enable it within a Drupal theme at **Site Building | Blocks | List | ThemeName** by specifying a region for the **CiviCRM Upcoming Events** block and clicking **Save blocks**. Additional configuration options for **Link name to display** and **Max number of events to display** are available by downloading and enabling the CiviCRM Event Block module from http://drupal.org/project/civicrm_eventblock.