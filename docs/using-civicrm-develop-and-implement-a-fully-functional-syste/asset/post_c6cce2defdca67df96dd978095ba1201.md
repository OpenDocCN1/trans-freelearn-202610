
However, we strongly discourage deleting financial data transactions and will suggest some ways to prevent any users from having permission to delete financial transactions. A better practice is to post reversing transactions or use the status to communicate why the original transaction has changed (for example, by marking as cancelled and providing an explanation for the action).

One exception may be when you clean out test records from your system. In general, test records are well-hidden from the interface. You must explicitly choose to include them from the Find Contributions tool (check Find Test Contributions?); otherwise, they will be excluded by default.

However, it is reasonable to want to periodically search for these test records and use the bulk Delete Contributions action to remove them from the system. Be sure you have a recent backup of your database before performing any bulk deletion of this sort.

As you can see, CiviCRM provides a number of powerful tools for working with contribution records in bulk. Take time to explore the features available so that you know your options when you are working with records and have a clear understanding of what they do.

## Searching, examining, and taking action on pledges

When compared to contributions, fewer options related to pledge management are available. As described earlier, a pledge is a collection of contribution records (pledge payments) defined by the encompassing pledge options.

### Searching pledges

To search for pledges, click on Search | Find Pledges or Contributions | Pledges | Find Pledges. The following criteria are available when searching pledges. We will note any particularly useful, interesting, or possibly unexpected behavior when using the criteria:

- **Pledger Name or Email**: Search using a partial string for the pledger name or e-mail.
- **Payment Scheduled – From and To**: Enter a range, or just a start or end date. Like other fields, not entering a value here will return records regardless of the value in this field.
- **Pledge Payment Status**: One or more of Completed, Pending, Cancelled, Overdue.

- **Pledge Amounts – From and To:** These amount ranges are inclusive – it will include pledge amounts equal to or greater than **From** and equal to or lesser than the **To** amount. When using any of the range based fields, you do not need to provide a value for both. For example, entering **50** in the **Pledge Amount From** field will return all pledges greater than or equal to 50.
- **Pledge Status:** One or more **Completed, Pending, Cancelled, In Progress,** or **Overdue** statuses. Understand the difference between the **Pledge Payment Status**, which tracks individual payment records, and the **Pledge Status**, which tracks the overall status of the pledge record.
- **Pledge Made – From and To:** These are inclusive – pledge amounts equal to or greater than From and less than or equal to **To**.
- **Payments Start Date – From and To:** These are inclusive of pledge amounts equal to or greater than **From** and less than or equal to **To**.
- **Payments Ended Date – From and To:** These are inclusive of pledge amounts equal to or greater than **From** and less than or equal to **To**.
- **Contribution Type:** Enter one or more contribution types.
- **Contribution Page:** Note that this list may include contribution pages that are not currently configured to allow pledges. For example, there may have been a time when they allowed pledges and pledges were made against them.
- **In Honor Of:** Note that it searches all or part of first and last name, but does not search the e-mail address.
- **Find Test Pledges?** If selected, only test pledges are returned. If unselected, only non-test pledges are returned.
- **Pledge Frequency Every [duration] [frequency]:** Retrieves pledges defined by the selected frequency options.

The search results return the "container" pledge record, but you may click the arrow to the left of the record to expand and view the associated pledge payment schedule.

### Examining pledges

Pledges are more complex than single contribution records. In essence, they are groups of contributions bound and managed as a single unit (with distinct elements). The creation of the pledge defines the contribution records; the status of the individual pledge payments controls the status of the overall pledge record.

After conducting a search, the pledge search results page allows easy drill-down access into the schedule of payments, both pending and completed, that collectively make up the pledge record. In addition to View/Edit/Delete links, a Cancel action is available for pledges. Canceling, unlike deleting, doesn't remove the records of payments received. It is the preferred way of managing pledge records that have been stopped mid-payment.

Each payment revealed in the drill-down has action links on the right that allow a payment to be recorded by the administrator and also allow easy editing of the payment schedule when there is an issue with a payment. For example, if a payment has to be reduced or the due date extended, a single pledge payment record can be adjusted and the remaining payments will be adjusted accordingly.

### Taking action on pledges

There are two actions available as operations on search pledge results: pledges can be exported or deleted. The interface and workflow are very similar to those for contributions described above and for other objects such as contacts, events, and memberships described elsewhere. After conducting your search, select some or all pledge records, choose an action, and click on Go.

#### Export Pledges

Select Export PRIMARY fields and click on Continue to save a .csv file containing primary contact and contribution fields to your local hard drive. The export can be used for importing into your accounting system, sending mails, or whatever other needs you have.

If you want more control over what fields are exported, choose Select fields for export and click on Continue to advance to the field selection page. From this page, you will be able to specify fields to be exported and, if desired, save the field mapping for future use. Once you have selected your fields, click on Export to generate and download the export file. Refer to your accounting system documentation for the fields it can make use of through its .csv import functionality.

If you have previously saved an Export field mapping, it will be available for selection and re-use when you choose the Select fields for export option. After selecting an existing field mapping and clicking on Continue, you will be able to review and modify the list of exported fields, and be provided the option to Update this field mapping or Save as a new field mapping. Selecting the former updates the field mapping when Export is clicked. Selecting the latter allows you to provide a Name and Description, and then saves the mapping as well as exports the data when Export is clicked. You can leave both unselected and just click Export to export the data without modifying the existing field mapping.

#### Delete Pledges

As the action title suggests, initiating this option on pledge search results will remove them along with their associated contribution records from the system. This is a dangerous function, particularly when working with contribution records which have certain data integrity and retention obligations.

To reiterate, we strongly discourage deleting financial data transactions, and even suggest ways to prevent any users from having permission to delete financial transactions. If a payment (or pledge) has been made, it is better to cancel the record (retaining its history) or post reverse transactions (indicating that the funds were returned) than to delete it from your system. Deleting it altogether results in loss of historical data — the details and information about what transpired with the transaction. That is a poor business practice and may not adhere to generally accepted accounting principles.

The one exception to this rule is with test records that were generated solely for the purpose of testing functionality, and whose financial charges (if any) have been reversed. In the pledge search tool, select Find Test Pledges to retrieve them and use the Delete Pledges action to remove them.

## Reporting

You can create contributions and pledges, manage existing records, conduct robust searches, and take bulk action on the results; but how do you translate the wealth of data in the system into a meaningful report for organization leadership? How do you turn raw data into a story that tells people where the organization is and where it is headed? In this section, we will translate some of the search tools described above into meaningful examples of how you might use them to extract important statistics and report summaries.

### Counting prospects with Advanced Search

When trying to determine the number of prospects for different segments, you need to make sure to count contacts rather contributions. For example, if you want to find the number of people who have given a donation between $100 and $500, you would not use Contributions | Find Contributions or Search | Find Contributions because it may return duplicate contact records if the contact has given more than one donation within that range.
