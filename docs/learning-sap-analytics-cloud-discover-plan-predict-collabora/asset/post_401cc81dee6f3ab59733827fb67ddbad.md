
| Name | Create | Read | Update | Delete | Execute | Maintain | Share | Assign | Notes |
|---|---|---|---|---|---|---|---|---|---|
| Public Files | X | X |  | X |  |  |  |  | Permits access to public files. |
| Private Files | X | X |  | X |  |  |  |  | By default, the Create permission is granted to all roles except the Viewer, BI Content Viewer, and SAP CP Content Viewer. Without this permission users cannot create stories, upload data into a story, or upload other local files from their computer. |
| Ownership of Content |  |  |  |  | X |  |  |  | Users with this permission can transfer the ownership of content to another user when a user is deleted. |
| System Information |  | X | X |  |  |  |  |  | Set Read to provide access to the System \| Monitor area. Set Update to additionally provide access to the System \| Administration area. |
| Allocation Step | X | X | X | X | X |  |  |  |  |
| Allocation Process | X | X | X | X | X |  |  |  |  |
| Pool Mapping | X | X | X |  |  |  |  |  |  |

And the following screenshot completes our list:

| Name | Create | Read | Update | Delete | Execute | Maintain | Share | Assign | Notes |
|---|---|---|---|---|---|---|---|---|---|
| Explorer |  |  |  |  | X |  |  |  | Set Execute to provide access to the Data View mode in a story. |
| Personal Data Acquisition |  |  |  |  | X |  |  |  |  |
| Value Driver Tree | X | X | X | X | X |  |  |  |  |
| Automated Discoveries |  |  |  |  | X |  |  |  |  |
| Boardroom Agenda | X | X | X | X |  |  | X |  |  |

## Test role permissions

When you create the account of a new user and assign her relevant roles, the user receives an email that provides her the username, initial password, and a link to access SAP Analytics Cloud. When she successfully logs in, she is prompted to reset her password. After fulfilling these formalities, when she goes to the Models area, she gets the following screen. Note that the only model she can access is Sales Marketing Plan due to the permissions assigned to her in the previous section:

![](img/c69568baeee565fe02397867c05883e1_372_0.png)

## Update user profile

Users can update their own profile, which includes adding a picture, entering Job Title, E-mail address, and Function Area, as shown in the following screenshot. Users can access their respective profiles by clicking on their default picture icon and selecting Profile (A) from the menu:

![](img/c69568baeee565fe02397867c05883e1_373_0.png)

## Forming teams

A team is a group of users who perform a similar type of task. In SAP Analytics Cloud, you can group individual users together to create teams. There are many benefits to forming teams. For example, you can share a story with all the members of a team without having to select users individually. Similarly, you can create a role and assign it to a team. If you make any changes to the permissions set for the role, the changes are inherited to the team and consequently to each member of the team. Here are the steps to create teams:

- 37. Go to Main Menu | Security | Teams to access the Teams page, as illustrated in the following screenshot. The Teams page lets you add, modify (A), and delete (B) a team.
- 38. Click on the Create Team icon on the toolbar to form a new team.

![](img/c69568baeee565fe02397867c05883e1_374_0.png)

- 39. Enter a unique name in the Team Name box. Spaces are not allowed in the team name. You can use uppercase and lowercase letters, numbers, underscores, hash mark, and ampersand.
- 40. If required, enter a Description to further elaborate the team.
- 41. Click on the Add button to select the team members from the ensuing Select User dialog (A). When you are done, choose OK to return to the Create Team dialog.

The control provided in the Members area can be used to add or remove users from the list. Use the Search control to find a particular user if your list is long. Click on the Create button to finish the team creation process.

Now the members of the team can share their stories and files with each other by selecting the team name in the Select Users and Teams dialog instead of individual members. The team members can find the shared story under the Shared with me tab on the Stories page. For further details, refer to the Sharing a Story With Colleagues section and Sharing Files and Other Objects section in Chapter 8, Collaboration.

### Create Team

### Select User

Team Name *

Sales_and_Marketing

Description

West U.S. Sales Team

Members

Search

- ☑ Bulent
- ☑ Dalia
- ☑ Sheela

Create

Cancel

Search

- ☑ Bulent
- ☑ Dalia
- ☑ Sheela
- ☐ Sophie

39

40

41

A

## Transferring the system owner role

One user must always be assigned the system owner role. The user who possesses this role can access all areas of the application without any restrictions. This role is transferable. If, for any reason, you want to transfer this role, you can do that. You must be logged in as a user with the system information update privilege. By default, the system information update privilege is enabled for Admin, BI Admin, System Owner, and BI System Owner.

Here are the steps to transfer this role:

- 42. Go to Main Menu | Security | Users.
- 43. On the Users page, select the user you want to assign the system owner role.
- 44. Click on the Assign as System Owner icon on the toolbar. The Transfer System Owner Role dialog appears, which shows the selected user as the new system owner and also displays the name of the Previous System owner section.
- 45. Under New Role, click on the icon to open a list of available roles. Select one or more roles for the previous system owner, and click on OK in the Select Roles dialog:

![](img/c69568baeee565fe02397867c05883e1_376_0.png)

- 46. Click on the OK button on the Transfer System Owner Role screen to complete the process.

The user you selected is assigned the system owner role, and the previous owner is assigned the role you selected:

|  | USER ID | FIRST NAME | LAST NAME | DISPLAY NAME | E-MAIL | MANAGER | ROLES |
|---|---|---|---|---|---|---|---|
| 1 | ADAM | Adam | Smith | Adam | adam.smith@sap.com |  | System_Owner |
| 2 | VICTORIA | Victoria | Swanson | Victoria | v.swanson@sap.com |  | Admin |

## Setting security for models

You can also make your models secure. The following options are available for this purpose:

**Model security**: You can make your model secure by turning on the Privacy option (A) in model preferences. When this option is turned on, the model is accessible only to the owner and other users who are explicitly granted access through roles by the administrator:

![](img/c69568baeee565fe02397867c05883e1_378_0.png)

**Dimension security**: You can also apply security at the dimension level by enabling Data Access Control (B). With this type of security, access to individual values in the model output can be restricted to specific users. You can apply this security to a new dimension (C) or to an existing dimension by selecting it and clicking on Preferences (D). When you enable Data Access Control, the Read and Write columns are added to the dimension. Using these columns, you can specify the users you want to grant permission to in order to read or write the values for that dimension:

![](img/c69568baeee565fe02397867c05883e1_378_1.png)

**Roles-based security**: You have already applied role-based security in the previous section through permissions. When you access the definitions of a role, you see the Permissions screen, where you can grant permission to access specific functional areas of
the application including models. In addition to providing permission on individual items, you can grant read or write access to selected models and model dimensions using the same interface. For models that have the Privacy feature enabled, you can apply some more granular security features.

Here are the steps:

- 47. Click on the Select Model menu, and choose the Select New Model option.
- 48. In the Select New Model dialog, choose a model with privacy enabled. The name of the selected model will appear on the second tab, and the screen will be transformed to define permissions for the model. You have the option to grant Full Access or Limited Access to the role. After selecting an option from this radio group, you have to click on either the Define Read Access or Define Write Access buttons. Clicking on any of these buttons displays another window, where you can specify a detailed set of permission rules based on all members and categories of the model. For example, you can add an entry that provides read access to the role only for the Actuals category of the model.

![](img/c69568baeee565fe02397867c05883e1_379_0.png)

## Keep a vigilant watch

Keeping an eye on the system is indeed the most important task of an administrator, especially in a multi-user environment. In SAP Analytics Cloud, you, as an administrator, can monitor users activities, data changes, and system usage.

![](img/c69568baeee565fe02397867c05883e1_379_1.png)
