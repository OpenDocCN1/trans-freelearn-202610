
| Sr. No. | Checkpoint | Expected Result |
|---|---|---|
| 1 | Include a maximum of seven items in the Favorites. Is this standard followed? | Yes |
| 2 | More than two nested (indentation) levels must not be used. Is this standard followed? | Yes |
| 3 | The criteria for grouping the items under favorites must be **type of task**. Is this standard followed? | Yes |
| 4 | Clear, concise, and easy-to-understand names must be used for Favorites. Cryptic abbreviations or jargon must not be used. Is this standard followed? | Yes |

## Main Menu

The Main menu plays a pivotal role while navigating to various forms, reports, etc. It is designed in such a way that it increases the usability (by structured navigation), and shortens the navigation path (i.e. decreases the average number of clicks to reach various forms/reports). In order to preserve these qualities any customization must conform to the following specifications:

| Sr. No. | Checkpoint | Expected Result |
|---|---|---|
| 1 | Does the number of forms at the start of the menu (for any module) exceed six? | No |
| 2 | Is there any menu in which forms shown at the start are infrequently used? | No |
| 3 | Is there any folder icon that is named other than Journals, Inquiries, Reports, Periodic, and Setup? | No |
| 4 | Do the folder icons appear in the order of Journals, Inquiries, Reports, Periodic, and Setup? | Yes |
| 5 | Is there any folder that contains more than six forms? (A folder not containing any form is acceptable.) | No |
| 6 | Is there any node in the main menu whose indentation level is more than 5? | No |
| 7 | Is there any menu icon that is inconsistent with Dynamics AX standards? | No |
| 8 | If a user clicks a form icon on the main menu, make sure that the same form icon appears in the upper-left corner of the new window. Is there any window in which this standard is not followed? | No |
| 9 | Is there any form icon, whose title is different from the title on its corresponding form? Note: Form title may contain an extra string at the right but it must start with the label string mentioned in the form icon. | No |
| 10 | Is there any form that does not open by single-clicking the form's icon Note: sometimes opening a form may take time after clicking the icon, the tester is advised to test it patiently. | No |
| 11 | Does double-clicking the form's icon open two identical forms? | No |

## Task Pane Requirements

The task pane appears (if you opt to display it) at the right side of the application window. The disadvantage associated with it is that it reduces the space for the content pane and hence many users opt to not display it. Hence, UI specifications recommend using the task pane in a way in which users are not compelled to opt for it to complete their task. A few suggested uses of the task pane may be to display the search results, help contents, etc. The following specifications must be observed to ensure that a user is not compelled to use the task pane to complete any task:

| Sr. No. | Checkpoint | Expected Result |
|---|---|---|
| 1 | The task pane must be used to present information that is useful but not required. The task pane must not be used to display notifications or information that is critically important to the user's task. Is this standard followed? | Yes |
| 2 | The task pane must not be used as a dialog box or for setting options. Is this standard followed? | Yes |
| 3 | The task pane must not be used as a secondary workspace that competes with the content pane. The content pane is the primary workspace for displaying forms and windows associated with application tasks. Is this standard followed? | Yes |
| 4 | The task pane must not be used to present excessive information that confuses the user. Is this standard followed? | Yes |
| 5 | The task pane must not be used to display content that is specific to the currently selected form or window. The task pane should display information that applies to the application in general or that is peripherally useful. Is this standard followed for every window? | Yes |
| 6 | Consider using a Wizard or alternative content format to present information or guidance. Is there any window in which this standard is not followed? Note: This is a recommendation so this checkpoint may be ignored with due justification. | No |

## Forms

Carefully designed forms contribute the largest part to usability, as most of the time users deal with forms only. Dynamics AX categorizes forms in three categories and all user interface specifications must conform to a particular form category.

| Sr. No. | Checkpoint | Expected Result |
|---|---|---|
| 1 | A form must fall in one of the three standard categories of forms i.e. Two tab controls (i.e. Header-line), One tab control, or Function window. Any combination of the above mentioned categories in a single form is not allowed. Does any such form exist? | No |
| 2 | Is there any two-tab control form that does not contain a movable splitter to split the form into header and lines? | No |
| 3 | The area above a table must either be blank or should contain one or more controls that are related to frequently used filters or presentation formats for the records displayed in the table. Is there any form in which this standard is ignored? | No |
| 4 | Is there any filter control that is supposed to filter records based upon the group header? | No |
| 5 | Filtering criteria for a table must not be provided in a tab under the same tab control. For example, a tab control containing a table must not contain a Filter tab that controls the filtering of the table. Is there any form in which this standard is ignored? | No |
| 6.1 | Inactive filter criteria must be blank. Is there any form in which this standard is ignored? | No |
| 6.2 | Is there any checkbox that can be used to activate or deactivate the filter controls? | No |

### Best Practices – GUI

| Sr. No | Checkpoint | Expected Result |
|---|---|---|
| 7 | The area below a table must either be blank or should contain any one of the following: Row extension controls Relevant summaries of the table, for example column totals Row extension controls contain information that belongs to the currently selected row. Row extension controls must be used to show all relevant fields, otherwise horizontal scrolling would be required to show all relevant fields. Row extension must be used only for fields where vertical scanning of the corresponding column is not relevant for users. Is there any form in which this standard is ignored? | No |
| 8.1 | A column total must total for the whole column in the database and not only for the visible part. If filters are active for a column, the total should be calculated for records controlled by filter criteria. Is there any form in which this standard is ignored? | No |
| 8.2 | A column total must total for the entire column in the database controlled by filters. Is there any form in which this standard is ignored? | No |
| 9 | In the default appearance of a table, horizontal scrolling must not be present. Is there any form in which this standard is ignored? | No |
| 10 | Forms of type 1 and 2 (described in checkpoint 1 for forms) must not contain OK, Cancel, or Help buttons. Is there any form in which this standard is ignored? | No |
| 11.1 | Buttons on forms must be placed on the right-hand side in a column. Is there any form in which this standard is ignored? | No |
| 11.2 | No button should be tab specific. Is there any form in which this standard is ignored? | No |

## Edit Controls

Following are the specifications for edit controls. All edit controls including disabled edit controls must comply with these specifications.

| Sr. No. | Checkpoint | Expected Result |
|---|---|---|
| 1 | Are default edit labels overridden? | No |
| 2 | Fixed labels must not be used. In other words, all static text in a window (headers, instructions, edit control labels, etc.) must be taken from the Dynamics AX label file system to facilitate localization. Is there any edit control in which this standard is ignored? Note: This can be checked during Dynamics AX best Practice Check tool so this checkpoint can be ignored here | No |
