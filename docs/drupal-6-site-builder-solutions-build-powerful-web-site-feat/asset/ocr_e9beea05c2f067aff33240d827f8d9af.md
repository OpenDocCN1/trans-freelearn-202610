| Enabled | Name | Version | Description |
|---|---|---|---|
| ☑ | **Date** | 6.x-2.0-rc4 | Defines CCK date/time fields and widgets. Depends on: Content (disabled), Date API (disabled), Date Timezone (disabled). **Required by:** Date Copy (disabled) |
| ☑ | **Date API** | 6.x-2.0-rc4 | A Date API that can be used by other modules. Required by: Date (disabled), Date PHP4 (disabled), Date Popup (disabled), Date Repeat API (disabled), Date Timezone (disabled), Date Copy (disabled) |
| ☐ | **Date Copy** | 6.x-2.0-rc4 | Import and export CCK date data. Depends on: Content (disabled), Date (disabled), Date API (disabled), Date Timezone (disabled) |
| ☐ | **Date PHP4** | 6.x-2.0-rc4 | Emulate PHP 5.2 date functions in PHP 4.x, PHP 5.0, and PHP 5.1. Required when using the Date API with PHP versions less than PHP 5.2. Depends on Date API (disabled) |
| ☑ | **Date Popup** | 6.x-2.0-rc4 | Enables jQuery popup calendars and time entry widgets for selecting dates and times. Depends on: Date API (disabled), Date Timezone (disabled) |
| ☐ | **Date Repeat API** | 6.x-2.0-rc4 | A Date Repeat API to calculate repeating dates and times from iCal rules. Depends on Date API (disabled) |
| ☑ | **Date Timezone** | 6.x-2.0-rc4 | Needed when using Date API. Overrides site and user timezone handling to set timezone names instead of offsets. Depends on Date API (disabled). Required by: Date (disabled), Date Popup (disabled), Date Copy (disabled) |

+   4. Now that the appropriate Date modules have been enabled, we can add our new field. The start and end dates are implemented as single **Date** fields using the *jQuery pop-up calendar*, with the ending date being required if a starting date is entered.
+   
+   5. Begin as we did for adding the Price field and seasonal fields, by clicking on the **Add field** link. This time, we will name the field **Seasonal Dates** and set the **Widget type** to **Text Field** with *Date Pop-up calendar*. Click **continue** to add the field and enter the advanced information.