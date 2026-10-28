
Before configuring the grant tools, let's first understand the basic process assumed by CiviCRM as it relates to grant workflows. The following diagram helps visually communicate the workflow for organizations giving or receiving grants:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_416_0.png)

The grant application process basically consists of receiving applications, processing them to determine if they are to be approved or denied (that is, granted or accepted), and then tracking the progress as funds are transferred and reports are received from the grantee.

Grant records, like most other areas of CiviCRM, can have custom fields created and attached to them. In this way you can set up as many additional fields as desired to help in the grant-making or grant-seeking process. The ability to attach documents to records in CiviCRM is particularly useful when it comes to grant application documents. It is also useful for attaching scanned copies of official correspondence.

Grants are categorized by type allowing you to search for and track the status of different grants organized by type. Before beginning the grant management process you should review the existing grant types configured by default and add/edit/delete as necessary. You can access them at Administer | CiviGrant | Grant Types.

If the CiviGrant submenu is not visible, make sure you have the CiviGrant component enabled. Visit Administer | Configure | Global Settings | Enable CiviCRM Components. By default, the CiviGrant component is disabled. Enabling it will restore the CiviGrant submenu under Administer, and also the main Grants submenu (under Other).

Once you've thought through the grant types, created any custom fields you want associated with grants, and reviewed the general grant workflow, you can begin managing your grant applications.

## Managing grantees

As with most areas of CiviCRM, your grant management tools will be available through the main menu and a corresponding tab will be displayed on contact records. By default, the Grants menu is a submenu under Other. If you expect to be a heavy user of the grant tools, you may want to move it to the main menu. Visit Administer | Customize | Navigation Menu to edit the main horizontal menu.

Let's begin by visiting a contact and creating a new grant record.

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_417_0.png)

As you can see, the grant record follows the flowchart workflow pretty closely. You select the desired status, type, and complete the other fields as appropriate.

## Providing Support: Grant Management

Grant records are a little different from other areas of CiviCRM in that you are likely to return to edit the record multiple times over the course of the grant process. As the grant moves through the application, approval, fund dissemination, and reporting stages, you will return and edit the record to update the dates and record notes or add attachments pertaining to the grant.

As with other related record areas, contacts may have more than one grant record associated with them.

> FPAGM received a significant donation from a local philanthropist to help food pantries develop new healthy eating programs. The programs will educate clients regarding healthy eating and provide guidance on simple recipes using ingredients rather than processed, prepared foods.
> The donation will support five programs at food pantries in the area. FPAGM decides to use CiviCRM to track the grant application and management process. They begin by sending out an e-mail to Regular members through CiviCRM with application guidelines. As the applications are received, a new grant record is created.
> The application requires food pantries to report details regarding their geographic demographics, and to describe how many people they plan to reach through this program. All of this data is tracked using custom fields, and the hard copy application is scanned and attached to the grant record. In this way, FPAGM ensures that all the details of the process are stored in a centralized location.

The following screenshot shows the grant records as viewed through the contact's Grants tab:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_418_0.png)

## Tracking grant applications

Let's take a look at the tools available in the **Grant** submenu for tracking grant records. As you might expect, **Grants** follow a similar pattern as other areas by having a dashboard summary of grant statistics, a **Find Grants** tool, and a link to quickly create a new grant record.

The dashboard summarizes the number of grants by status (with each cell hyperlinked to display a filtered list of records), and a list of recent grant records created.

**Find Grants** provides access to each of the relevant data fields, including ranges for the grant amount and date fields, as shown in the following screenshot:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_419_0.png)

The date range fields provide the additional option of searching by an empty value. For example, if you are looking for an updated list of applicants who have not yet received a funds disbursement, you can select a status of **Granted** and **Money Sent Date** of "not set", and run your search.

Grant records may be exported to a CSV file after running your search.

## Other side of the coin: Applying for grants

Although the CiviGrant component is primarily intended for the grant administration process, presuming that the organization implementing CiviCRM is the grantor disseminating funds, it's worth noting that the grant tools can be adapted easily to manage your organization's applications for grants from other entities.

In this alternate case, the contact record you are working with is the grantor, rather than the grant applicant and potential grantee. You are using CiviGrant to track your application to the organization and subsequent communication transpiring over the course of the process.

Since the workflow is essentially identical regardless of which direction you are coming from in the process, we simply need to alter terminology throughout the interface to better communicate your role as a grant applicant. There are two ways this can be accomplished — by modifying the CiviCRM template files, or using the word replacement tool. As a general rule, it is better to use the word replacement tool to the extent possible, as it is more dynamic and avoids having to maintain custom versions of template files. Use the tool to define existing text and replacement text; wherever the existing text is found in the system, it will be replaced. At times you may find text that is not easily isolated (for example it is too generic or used elsewhere in the system) and cannot be replaced universally. CiviCRM uses the Smarty Templating System to present content and provides the ability to define an override directory in which replacement template files are stored. Using template overrides you have full control over the text and layout of your pages. Refer to *Chapter 3, Customization, Community, and Cooperation* for more tips on customizing the interface in this way.

- Using word replacements, we see several places in the interface that would benefit from being changed to better communicate this alternate grant application perspective. Visit **Administer | Option Lists | Word Replacements**, and then enter the following:

## Word Replacements

Use Word Replacements to change all occurrences of a word or phrase in CiviCRM screens (e.g. replace all occurrences of 'Contribution' with 'Donation').

| Enabled | Original | Replacement | Exact Match? |
| --- | --- | --- | --- |
| ☑ | Application Received | Application Submitted | ☑ |
| ☑ | Grant Report Received? | Grant Report Submitted? | ☑ |
| ☑ | App. Received Date | App. Submitted Date | ☐ |
| ☑ | Grant report received? | Grant report submitted? | ☑ |

- After you have entered the changes, click on Save. Check if the interface has changed correctly by navigating to Other | Grants | New Grant and Other | Grants | Find Grants.

If you regularly apply for grants to different organizations and find the tools available through the grant component to be too limiting, consider using CiviCRM's Case Management tools by creating a case type to track communication with the grantor.

As covered earlier in this book, case records consist of a collection of activity records grouped together around a common issue. Since a grant application is primarily a series of communications and reports passing between the grantor and grantee, a case may serve as a suitable alternative to the grant component.

## Summary

The opportunity to support other organizations through financial assistance is coupled with the responsibility to administer those funds and application details carefully. Your funders will expect that you will take as great care administering funds as your grantees do expending them. Using CiviCRM, you have effective tools to handle this process as a part of your overall constituent management program.

In this chapter, we:

- Reviewed the grant management workflow and discussed how it translates to the CiviCRM model.
- Created new grant records and saw how each step in the process is tracked.
- Searched for grants and noted the various field options unique to these records.
