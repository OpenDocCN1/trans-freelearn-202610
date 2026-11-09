# Tree Views

Tree view is a control in Dynamics AX applications. It must conform to the following specifications:

| Sr. No. | Checkpoint | Expected Result |
|---|---|---|
| 1 | Are Tree views used where hierarchical information must be presented? | Yes |
| 2 | Each line in a tree view must contain an icon followed by relevant information about the object. Is there any tree view in which this standard is ignored? | No |
| 3 | The information may be a comma-separated list. If it is a comma-separated list, the first two items in the list must be the object key and the object name. Is there any form in which this standard is ignored? Note: Comma-separated lists must not be used in the main menu even though it is a tree view. | No |
| 4 | Is there any tree view in which the separator between items is other than a backslash, a comma, or a slash? | No. |
| 5 | If more than two items are allowed in the comma-separated list, users must be able to select relevant items for the list in a tab; this tab must be named Setup. Is there any form in which this standard is ignored? | No |
| 6 | Does tree view support drag and drop for repositioning, adding and removing objects? Note: It will depend upon requirements so testers can exclude this checkpoint. | Yes |
| 7 | There must be an easily accessible explanation of the icons used in the tree view. The explanation must appear either on the Overview tab (or Tree tab if it is available instead) or on the Setup tab. Is there is any form in which this standard is ignored? | No |
| 8 | Is the form resized when tree view is selected or deselected? | No |
| 9 | If a user resizes a window containing a tree view, any increase in window size must be used to increase the size of the tree view. Is there any form in which this standard is ignored? | No |
| 10 | Is drag and drop (when available) described in the online help for the form? Note: If drag and drop is not available in any tree view control, this standard is not applicable. | Yes |
| 11 | The separator between items in a comma-separated list should be a backslash. Is there any tree view where this standard is ignored? Note: This is recommended and not mandatory so if justification is available, a tester can ignore this checkpoint at his or her discretion. | No |