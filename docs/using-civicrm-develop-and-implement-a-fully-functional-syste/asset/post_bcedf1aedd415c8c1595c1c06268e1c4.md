
## Pledge report templates

The pledge report templates are as follows:

- **Pledge Report**: Displays pledge details filtered by date, amount, or status.
- **Pledged But not Paid Report**: Displays contacts who have pledged, but have outstanding or overdue payments.

## Case report templates

The case report templates are as follows:

- **Case Summary Report**: Provides a summary of cases and their duration by date range, status, staff member and/or case role.
- **Case Time Spent Report**: Aggregates the time spent on case and/or non-case activities by activity type and contact.
- **Contact Demographics Report**: Displays demographic breakdown for case clients (and/or non-case contacts) in your database. It also includes custom contact fields.

## Grant report templates

The grant report templates are as follows:

- **Grant Report**: Generates report filterable by any of the grant data fields, including date range options.

> A few report templates come in a disabled state. Visit **Administer** | **CiviReport** | **Manage Templates** to view and manage the status of report templates registered in your system.

FPAGM staff members deal with several levels of leadership and key stakeholders who expect to receive reports on a regular basis. This includes the Executive Director, who is concerned with membership numbers and total donations, the Marketing Committee chair, who requires monthly membership status reports, and the Development Committee Chair, who wants to see total contributions of major donors over specified periods of time.

During system configuration, for each requested report, they review the list of templates and select and configure the one they believe will be most appropriate for their needs. Since the Executive Director has much on his/her plate, he/she look for reports that support bar and pie charts, as it provides a visual snapshot of the organization's activity that can be interpreted quickly.

The Marketing Committee chair wants to receive reports on a monthly basis. The staff members use the **Membership Report (Summary)** template to construct a report that breaks down membership by type, grouped by month, and displays data for the previous 12 months. The e-mail settings are configured and a cron job is set up to have this report e-mailed to the chairperson on the last day of the month, freeing staff from the responsibility of generating and sending the report.

Likewise, a "Top Donors" report is constructed, displaying the activity for the previous six months. Since this report is only provided as requested, staff members save it to the **Contributions** menu for easy access.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_435_0.png)

## Customizing and building your own templates

Though the reports that ship with CiviCRM seek to anticipate the bulk of your potential reporting needs, it is possible that you may have needs not addressed by existing report templates.

While it is beyond the scope of this book to delve into specific details regarding the development of new report templates, it is important to note that the report tools anticipate these needs. The code to build templates is very self-contained, modularized, and somewhat more accessible to beginner programmers than other areas of the software. You still need some familiarity with PHP and the MySQL database in order to build your own report template, but you may find it easier to work with than other areas of the CiviCRM codebase.

If you are interested in exploring this further, visit the following online documentation for guidance on customizing or creating a template: http://wiki.civicrm.org/confluence/display/CRMDOC/Custom+Reports+(How+to).

## Summary

Let's circle back to the original premise set forth at the beginning of this chapter:
*A report is primarily about telling a story—it is data with a message.*

We began this chapter thinking about what a report truly is, and guarding ourselves against missing the forest while staring at trees. After having reviewed the mechanics of creating and working with reports and outlining the various report templates available, it would be easy to lose sight of this broader context.

The reporting tools covered in this chapter are a means toward an end. You will select your report fields, define data filters, and choose your display options with the goal of communicating to other staff, organization leaders, or yourself, about the story of your constituents. This story takes shape through the lens of activities, contributions, pledges, memberships, events, cases, grants, or other windows into your data.

In this chapter, we:

- Set the context for working with reports and understanding the goal of reporting
- Defined key terminology used in CiviCRM reporting tools, including report templates and report instances
- Reviewed available report settings common to all reports
- Outlined the use of report criteria, which will be report-specific
- Summarized available report templates that are included with a base installation of CiviCRM
- Reviewed the core purpose and goal of reporting

## Customization, Community, and Cooperation

In this closing chapter, we want to tie up some loose ends and point you forward in your implementation of CiviCRM. We will learn about the following:

- Future versions and project roadmap
- Customizing and extending
- Community and cooperation

### Future versions and project roadmap

When we began writing this book, the current stable version of CiviCRM was 3.1.4 and 3.2 alpha was just being released to the community. Since then, v3.2 stable has been released and gone through several revisions, and 3.3 is now a stable release. By the time book production is finished, v3.4 will be nearing an alpha release. Needless to say, the CiviCRM development cycle typically follows the open source mantra of "Release early, release often".

This book is based on the v3.2 series. At times, we've provided comments where new v3.3 functionality impacts or expands the existing v3.2 toolset. However, for the most part we've focused on functionality present in the v3.2 release.

## New functionality

If you are new to CiviCRM and are introduced to a more recent version than v3.2, this book will still be of value to you, as the vast bulk of functionality is the same. The following represents key new features in v3.3 that we've not covered in this book:

- CiviCampaign is introduced as a new component in a somewhat beta version. The functionality is geared toward advocacy organizations and political campaigns, providing tools for canvassing, petitions, and surveys.
- **Advanced Search** can now be used to retrieve related records (not just contact records). Recall that when we reviewed search tools, we distinguished the different "lenses" through which records could be searched and retrieved. **Basic Search** and **Advanced Search** retrieve contacts (no duplicates) and component searches retrieve the related records (such as contributions). In v3.3, **Advanced Search** can now be used to retrieve these related records. For example, you can specify search criteria across contributions, memberships, and events, and retrieve contribution records.
- The dedupe/merge engine has been significantly reworked to improve performance. In previous versions, this tool could take significant time or would timeout if run across a large database. It will now scale quite well for large datasets.
- CiviMail has also received some scalability improvements and now supports multi-threaded sending. While dependent on the capabilities of your SMTP service, this can significantly increase throughput for large mailings. Several new reports have also been created to provide tools for searching across multiple mailings.
- A number of new reports for CiviCase and CiviGrant have been added, expanding the types of data that can be retrieved from these components.
- CiviEvent price sets now support a maximum value for field options. This was primarily intended to restrict the number of attendees signing up for breakout sessions. Many conferences have concurrent breakout sessions with a maximum number of attendees based on the size of the room. Each set of sessions will be a price set field, and each concurrent session an option value displayed as radio buttons with associated prices (if applicable) and maximum number of attendees.
- Do you recall the ability to share a household address with individuals discussed earlier in this book? In v3.3, this structure is extended to any contact type. For example, an individual could have their work address set to share the address of their employer.
