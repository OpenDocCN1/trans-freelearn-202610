The following screenshot shows one entity filled in. It also shows a section called **Custom details** where you can add your own custom entities. However, as of the time of writing this chapter, this custom entity does not show up anywhere.

To add a custom entity, just add a name for the field **Key** and select a value from the **Value** dropdown.

# Analytics rule wizard - Create new rule ...

# Entity mapping

Map up to five entities recognized by Azure Sentinel from the appropriate fields available in your query results.
This enables Azure Sentinel to recognize and classify the data in these fields for further analysis.
For each entity, you can define up to three identifiers, which are attributes of the entity that help identify the entity as unique. Learn more >

Unlike the previous version of entity mapping, the mappings defined below **do not** appear in the query code. Any mapping you define below will replace **not only** its parallel old mapping in the query code, but **any** mappings defined in the query code – though they still appear, they will be disregarded when the query runs. Learn more >

Account

FullName
Account
+ Add identifier

+ Add new entity

# Custom details

Here you can surface particular event parameters and their values in alerts that comprise those events, by adding key-value pairs below. In the Key field, enter a name of your choosing that will appear as the field name in alerts. In the Value field, choose the event parameter you wish to surface in the alerts from the drop-down list. Learn more >

Key
Value

+ Add new

Previous
Next : Incident settings (Preview) >

Figure 7.16 – Create new rule, Entity mapping and Custom details page

Now, scroll down the page and you will find some more fields related to **Query scheduling** and **Alert threshold**, as shown here: