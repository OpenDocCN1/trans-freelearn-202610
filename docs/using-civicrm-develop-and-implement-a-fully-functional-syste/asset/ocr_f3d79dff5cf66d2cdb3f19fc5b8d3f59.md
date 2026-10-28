# Collecting, Organizing, and Importing Data

Whether you are migrating from an existing database or starting from scratch, you will inevitably have additional data you need to store in fields. CiviCRM provides for this need through its custom data tools.

Custom data consists of **groups** and **fields**. You first define a group and assign it to an existing data object; you then create fields within that group.

By data object, we mean an existing type of record. Your custom fields will extend some type of record in your system, such as individual contacts, membership records, or groups. You configure what record type will be extended, along with any associated subtypes or options, when you create the custom data group.

Let's take a look at the custom data interface and walk through the creation of new custom fields to get a better sense of how this works. We can access custom data through **Administer | Customize | Custom Data.**

Begin by clicking **Add Set of Custom Fields**. Enter a name for the group and select how it will be used (what record type it will extend). This is the most important step in the process — once you begin collecting data in these fields, you will not be able to change the type of data it is connected to. Many of the options available will provide additional sub-options. For example, if you create a custom data group attached to events, you may then specify one or more event types that the data set will be used for (select **-Any-** to expose the custom fields to all event types).

![](bbox=[0.17, 0.5, 0.83, 0.73])

Let's briefly review the uses available:

+   - **Activities**: Available when creating or editing an activity record. You may further restrict its use to one or more activity types.