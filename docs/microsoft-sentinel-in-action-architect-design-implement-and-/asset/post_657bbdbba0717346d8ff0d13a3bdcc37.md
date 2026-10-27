
| Name | Description |
|---|---|
| Title | The name of the workbook. This should be descriptive enough so that users can easily tell what kind of information will be shown. |
| Subscription | Select the subscription where this workbook will reside. It should be the same subscription as your Log Analytics workspace. |
| Resource group | Select the resource group where this workbook will reside. It does not matter if it resides in the same resource group as your Log Analytics workspace. For instance, you could have one resource group for all resources related to your Microsoft Sentinel instance, or one resource group just for your workbooks, or a combination of both. Follow your corporate standards on where to place items if these apply. |
| Location | Select the location where your workbook will reside. You will want this to be the same location as your Log Analytics workspace to avoid egress charges. |

Table 8.1

Below that is a checkbox labeled Save content to an Azure Storage Account. If you need to protect the information stored in the workbook, you can secure it by saving the workbook's content to an Azure Storage account. See the Further reading section at the end of this chapter for a link to get more information on how to do this.

- 4. Click **Save** to save the workbook.

**Note**

It is not actually necessary for you to save the new workbook before you edit it. It is generally recommended that you do so to make sure you have a saved copy of it that you can revert to should your edits not work correctly.

That is all there is to it. Now, you will need to edit the workbook so that you can edit or remove the existing steps or add your own new steps. Refer to the next section, where we will cover more details on what can be done to modify your workbook.

## Editing a workbook

There will be times when you need to edit a workbook. As you saw in the previous section, you need to edit a workbook created from scratch to add what you need to it. You can also edit workbooks created from templates to modify them to suit your needs.

If you are not already viewing your workbook, you will need to view it first. If you are already viewing your workbook, you can skip this next step and move directly to the editing portion.

To edit a workbook, perform the following steps:

- 1. Go to either the **My workbooks** or the **Templates** tab.
- 2. Select the workbook in question.
- 3. Then, select the **View saved workbook** button in the workbook's detail pane.

> **Note**
> You cannot edit a workbook template directly. It must be saved first, and then the saved workbook can be edited. If you have created a workbook from scratch, you must go to the **My workbooks** tab since these workbooks have not been created from a template and only show up there.

At the top of the page is a header containing buttons, shown as follows. The one we care about in this section is the first one on the left, called Edit:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_252_0.png)

Figure 8.13 – Saved workbook header bar

The following table briefly describes each button:

| Icon | Name | Description |
|---|---|---|
|  | Done Editing | This button will change from edit mode to view mode. All the individual edit buttons will disappear. |
|  | Open | Open another dashboard. See the *Managing workbooks* section for more information. |
|  | Save | Save all the changes made to this workbook. |
|  | Save as | Save this workbook with another name. |
|  | Settings | Allows you to change some of the advanced settings for this workbook, including resources, styles, tags, and trusted hosts. The discussion of these topics is outside the scope of this chapter. |
|  | Revert changes | Discard any changes made and revert to the last saved version. |
|  | Refresh | Refresh the page. This will recalculate any queries on the page. |
|  | Share | Allows you to share this workbook directly with others. This will open a pane with the URL, which you can copy and send to others. Note that a workbook must belong to the Shared Workbooks group before you can share it. If the workbook is not shared, you will be prompted to move it to Shared Workbooks before you can share it. |
|  | Show pin options | This will open the pin options, where you can pin the entire workbook to a dashboard or pin each section that can be pinned to a dashboard. |
|  | Advanced editor | Switches the screen to show the JSON or Azure Resource Manager (ARM) code that gets generated automatically. See the *Advanced editing* section for more information. |
|  | Feedback | This allows you to provide feedback to Microsoft about workbooks. This works here as it does throughout the rest of the Azure portal. |
| ? Help | Help | Opens the Microsoft workbook documentation page in another tab. |

Table 8.2

If you look at the bottom of the workbook you are editing, you will see a list of links matching the following screenshot. This is how you will add new steps, and each step will be described individually in the *Workbook step types* section:

Add text | Add query | Add metric | Add parameters | Add links/tabs

Figure 8.16 – Edit mode add links

Once you have finished making all your changes, click on the **Done Editing** button in the header bar to revert to the view mode. All the individual edit buttons will disappear, as will any steps, parameters, or columns that have been set to be hidden.

Look at your workbook to make sure the edits you just made are working as desired. Once you are satisfied with your changes, click on the **Save** button to save your changes.

## Advanced editing

While the workbook's editing **graphical user interface (GUI)** allows you to completely create and edit a Microsoft Sentinel workbook, there may be times when you need to tweak a setting directly in the code. You may also wish to get the ARM template, which will allow you to easily reproduce this workbook elsewhere or store it as part of your DevOps process.

In either case, clicking on the **Advanced Editor** button will allow you to do that. When you click on the button, you will be taken to the **Gallery Template** view of the advanced editor, as shown in the following screenshot. This view will allow you to directly modify the JSON code. When you are done making the changes, click the **Apply** button to apply your changes, or the **Cancel** button to return to the GUI without saving your changes:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_255_0.png)

Figure 8.17 – Advanced Editor – Gallery Template view

Below that is a listing of all the workbooks. Clicking on any of the workbooks will change the workbook that you are viewing.

Each workbook will be shown in a separate row. It will display the name, and then an icon that will show whether the workbook has been created from a template, and then a context-sensitive menu. Clicking on the context-sensitive menu icon will show the menu as shown in Figure 8.20.

This menu will allow you to delete this workbook, rename it, move it to **Shared reports** (or if this workbook is already shared, it will allow you to move it to **My reports**), share it with others (or if it is not a **shared report**, you will be asked to make it a **shared report** before you can share it), and pin it to a dashboard, which provides a shortcut to get directly to this workbook. All of this can be seen in the following screenshot:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_258_0.png)

Figure 8.20 – Managing the workbook context menu

We have finished looking at how to manage your existing workbooks. You have learned about the Saved Workbooks pane, as well as the context-sensitive menu that will allow you to perform various management tasks on a workbook. Now, it is time to look at the various parts that make up a workbook, and how to use them.

## Workbook step types

Each workbook is comprised of one or more steps. As stated earlier, a workbook is made up of small sections called steps. Each step has a unique name, which can be pretty much anything, and this name can be referenced in other steps. Each step can run on its own, although some may require parameters, either from a parameter step or from other steps.

There are six different types of steps: text, query, metric, parameters, links/tabs, and groups. Each type of step will be discussed in more detail in the following sections.

To add a new step when editing a workbook, at the bottom of the screen is the +Add button, which, when clicked, will present a listing of the step types, as shown in the following screenshot. Click on the appropriate option for the type of step you wish to add:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_259_0.png)

Figure 8.21 – Edit mode Add option

No matter which links you click, the step will have a list of options at the bottom of the step that looks like the following screenshot:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_259_1.png)

Figure 8.22 – Edit mode add options after adding a step

Clicking the Done Editing button will change the selected step to view mode so that you can see how your changes look.

Click the Add button to add a new step.
