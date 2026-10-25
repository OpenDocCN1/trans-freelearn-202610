

There are a few important items to note about this screen. First, SugarCRM allows you to select from a wide variety of field types, including text, integer, drop-down, checkbox, and so on. For our example, we will choose `TextField`. Fill in the remaining fields with the following parameters:

`Data Type: TextField`

`Field Name: test`

`Display Label: Test`

Click on the **Save** button after entering the aforementioned parameters to commit the field. It is important to understand that although the field is now part of the accounts module (and database structure); you will not be able to see the field within the module until you place it on one of the layouts. Thus the process of adding a field is actually twofold, involving the definition of the field, and secondly, the placing of the field on the necessary layouts to make it available for editing or visible to the user. To complete the process, let us place the field on the EditView layout for the Accounts module.

## Customizing SugarCRM

Begin by expanding the **Layouts** folder below the **Fields** option on the left-hand side of Studio. Click on **EditView** and you should see a screen similar to the following:

![](img/7ccaf60360d7c8e809d336edbba2263d_325_0.png)

You must first find or create an empty **filler** area on the layout where you can place the field. To create a new **filler** area, you can simply drag the **New Row** component from the toolbox on the left to a desired area on one of the existing panels. The end result should resemble the following (the new row was inserted above the existing **Campaign** field):

![](img/7ccaf60360d7c8e809d336edbba2263d_325_1.png)

If you prefer to place the field on a new section or panel on the EditView, you will want to first use the **New Panel** component and place it on the EditView layout. Once you have created the new **filler** area, place the field on the view by dragging it from the toolbox and onto one of the empty **filler** areas.

You must now instruct SugarCRM to apply the changes to the system. To do so, click on the **Save & Deploy** button. You can verify that the changes were indeed applied by navigating to the edit view screen of an existing account or by creating a new account. In case you are wondering about the function of the **Save** button, it is used to save your modifications, but does not apply the changes to the system. This option is helpful for customizations that require a long time to complete and should not be deployed until fully completed.

If you take a look at the detail view screen of an account, you will notice that the new custom field is not visible. The reason for this is that you only added the custom field to the EditView layout. This example serves to highlight the point that the layouts work independent of each other. Adding or removing a field from the EditView does not affect the DetailView or the ListView and vice versa. To display the field on the DetailView, you must go back to Studio and modify the DetailView layout for the accounts module accordingly.

Other fields can be added using the same technique and if you would like to do so, you can also rearrange or remove already existing fields from the layout. To remove an item, access the appropriate layout in Studio, then drag the item from its current location over to the trash bin icon in the toolbox.

While working in Studio, you may have noticed a series of buttons at the bottom of the screen. They are used to access the additional customization tools, including **Module Builder** and **Dropdown Editor**. The Module Builder tool will be explored in detail later in this chapter. For now, let us examine the Dropdown Editor.

## Dropdown Editor

The Dropdown Editor is a very valuable tool for the Administrator. It permits the values in all of the drop-down boxes in the system to be edited. The options presented to the user may be edited, deleted, rearranged, or new values may be defined. For example, if you enter a new Account record, you will notice there are various options to select from for the Industry field, including Apparel, Banking, Engineering, and a variety of other choices, but these may not be applicable to your business. By using the Dropdown Editor you can manipulate the order in which the values appear, as well as enter new values, or modify and remove existing values.

## Customizing SugarCRM

To edit a drop-down list, click on the **Dropdown Editor** button in Studio and select a name of a drop-down list to edit. If you do not know which is the appropriate list that corresponds with the drop-down field you wish to edit, carry out the following steps to identify the drop-down list:

- Click on the **Studio** button
- Select the corresponding module containing the drop-down field in question
- Expand the folder and click on **Fields**
- Click on the entry corresponding to the field in question
- Note the value for the **Drop Down List** parameter

![](img/7ccaf60360d7c8e809d336edbba2263d_327_0.png)

Based on the preceding image, illustrating the settings for the industry field from the accounts module, the drop-down list to modify is named **industry\_dom**. Thus you will want to click on the **industry\_dom** list when you access the Dropdown Editor tool.

## Doing so will present the following screen:

![](img/7ccaf60360d7c8e809d336edbba2263d_328_0.png)

On the preceding screen, you can add, remove, or modify entries for the selected drop-down list. To add a value, scroll to the bottom of the page and type the value into the **Item Name** and **Display Label** fields, and then click on **Add**. You may be wondering why there are two fields if we are typing the same value into both. The reason for the dual field technique is that it allows you to define the value to store in the database and the value to display on the screen independently. A simple example of such usage would be a scenario where you might only want to store the letter Y or N in the database, depending on whether a user chooses **Yes** or **No**, instead of storing the entire value. In this example, Y would be the **Item Name** and **Yes** would be the **Display Label**. Modifications can be made by clicking on the pencil icon located on the far right of the entry, while clicking on the trash icon would delete the entry.

It is also possible to rearrange the order in which the items are displayed. To rearrange them, you can drag-and-drop them to a desired location or use the **Sort Ascending** or **Sort Descending** buttons at the bottom of the screen.

Once you have finished making your changes, click on the **Save** button at the top. Unlike modifications to the layouts, clicking on the **Save** button here not only preserves your changes, but also applies them to the system.

## Logic hooks

All the techniques for customizing SugarCRM that we have examined up to this point have relied upon the use of built-in tools and do not require programming expertise to use them.

Logic hooks will be our first endeavor involving customization capabilities that can only be accomplished with some programming expertise. Access to the server's file system facilitates the process, but is not required.

First and foremost, let us describe a logic hook so that you have a clearer understanding of its capabilities. A logic hook is a bit of a custom PHP code that allows you to extend SugarCRM functionality. The code that makes up a logic hook is invoked upon certain activities occurring within SugarCRM. For example, a user saving a record (existing or new), deleting a record, and other actions. In turn, the custom PHP code that those actions trigger can be programmed to do just about anything that is possible through PHP. Your main limitation would be your PHP skills.

It is important to also understand that logic hooks are designed to interact with the data, not the user and they are intended to be upgrade safe.

To further clarify this latter point, a logic hook can be programmed to perform an action, such as automatically taking a value from a record upon it being saved in SugarCRM and then copying it to a data warehouse on another server. It, however, cannot be used to prompt the user with an on screen alert or pop-up, to notify them of a problem with the copy process, its success, or any other sort of interaction. Again, the logic hook can interact with the data, but not the user.

For our example, we will create a logic hook that ensures uniformity in entering contact names within this SugarCRM system. More specifically, it enforces the fact that whenever a contact is saved, the first and last name values are always saved in proper casing. For example, if a user enters the value JOHN and DOE for the first and last name fields respectively, the values are to be stored as John and Doe upon the data being saved.

To accomplish this goal, we need to create a logic hook that automatically changes the value of the first and last name fields to proper casing. We must first create a logic hook definition file that contains information used by SugarCRM to determine not only the actions that cause the custom code to run, but also the location of the file containing the custom code that is to be run. This is a standard PHP file and can be created using your text editor of choice (Notepad, vi, and so on), but its name must always be `logic_hooks.php`.

