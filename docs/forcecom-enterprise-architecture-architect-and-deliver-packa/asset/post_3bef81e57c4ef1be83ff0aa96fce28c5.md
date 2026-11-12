
## Record relationships

A relationship is formed by a **Lookup** field being created from one object to another, often referred to as a parent and child relationship. There are two types of relationships in Salesforce: **Master-Detail** and **Lookup**.

> Custom Metadata Type objects only support Lookup relationships to other Custom Metadata Type objects or to other Custom Object or custom field metadata. Some features described in this section only apply to Custom Objects, such as lookup filters and referential integrity. Custom Setting objects do not support any form of record relationships.

In some aspects, they share similarities in how they are referenced when using SOQL and Apex and how they are input through the user interface (such as **lookup filters** described in the previous chapter). However, there are differences regarding **limits** and **referential integrity** features, and they are as follows:

- Utilizing the Master-Detail form of relationship provides a powerful way to model containment, where one parent object record acts as a container for other child records; in our example, a race contains **Contestants**.
  - The platform will automatically cascade deletion of child records when the parent is deleted, and it will also enforce the need for a parent reference when the child records are created.
  - By default, the platform prevents users from moving the child records to another parent by editing the relationship field. However, you can disable this validation and permit this by selecting the **Child records can be reparented to other parent records after they are created** option.
  - With the use of **Rollup Summary** fields, it also becomes possible to build calculated fields based on values of the child record fields.
  - Salesforce, however, does recommend that this kind of parent-and-child relationship does not exceed 10,000 child records; otherwise, the platform will not support the cascade delete of child records.
  - You can define up to three levels of Master-Detail relationships.

- Utilizing the **Lookup** type of relationship allows you to model connections between related records that are optional and may also have other relationships not necessarily in an ownership or containing aspects of their parents. Keep in mind the following points when using Lookup fields to define relationships:
  - Deletion of parent records will not result in a cascade delete of related child records; this can be enabled via Salesforce Support, though this setting will not be packaged.
  - Unlike with Master-Detail, you can elect for the platform to enforce referential integrity during the creation of the field by utilizing the **Don't allow deletion of the lookup record that's part of a lookup relationship** option.
  - Currently, the **Rollup Summary** fields are not supported when using a Lookup relationship.

Lookup fields of either kind are automatically indexed by the platform, making queries, in some cases, faster. We will focus more on indexes in a later chapter. The maximum number of relationship fields per object is 40.

> While it is possible to add new custom fields to objects that have already been packaged and released to your customers, you cannot add a new Master-Detail relationship to an object that has already been packaged.

## Reusing the existing Standard Objects

When designing your object model, a good knowledge of the existing **Standard Objects** and their features is the key to knowing when and when not to reference them. Keep in mind the following points when considering the use of Standard Objects:

- **From a data storage perspective:** Ignoring Standard Objects creates a potential data duplication and integration effort for your end users if they are already using similar Standard Objects as pre-existing Salesforce customers. Remember that adding additional custom fields to the Standard Objects via your package will not increase the data storage consumption for those objects.

From a license cost perspective: Conversely, referencing some Standard Objects might cause additional license costs for your users, since not all are available to the users without additional licenses from Salesforce. Make sure that you understand the differences between Salesforce (CRM) and Salesforce Platform licenses with respect to the Standard Objects available. Currently, the Salesforce Platform license provides Accounts and Contacts; however, to use Opportunity or Product objects, a Salesforce (CRM) license is needed by the user. Refer to the Salesforce documentation for the latest details on these.

Use your user personas to define what Standard Objects your users use and reference them via lookups, Apex code, and Visualforce accordingly. You may wish to use extension packages and/or dynamic Apex and SOQL to make these kind of references optional. Since Developer Edition orgs have all these licenses and objects available (although in a limited quantity), make sure you review your package dependencies before clicking on the Upload button each time to check for unintentional references.

## Importing and exporting data

Salesforce provides a number of its own tools for importing and exporting data, as well as a number of third-party options based on the Salesforce APIs; these are listed on AppExchange. When importing records with other record relationships, it is not possible to predict and include the IDs of related records, such as the Season record ID when importing Race records; this section will present a solution to this.

Salesforce provides Data Import Wizard, which is available under the Setup menu:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_126_0.png)

It is straightforward to import a CSV file with a list of race Seasons since this is a top-level object and has no other object dependencies. However, to import Race information (which is a related child object to Season), the Season and Fastest Lap By record IDs are required, which will typically not be present in a Race import CSV file by default. Note that IDs are unique across the platform and cannot be shared between orgs.

If you need a reminder of the structure of these objects, refer to the Schema Builder screenshot shown in Chapter 2, Leveraging Platform Features.

External ID fields help address this problem by allowing Salesforce to use the existing values of such fields as a secondary means to associate records being imported that need to reference parent or related records. All that is required is that the related record Name or, ideally, a unique external ID be included in the import data file.

The Races.csv file is included in the sample code for this chapter in the /data folder. This CSV file includes three columns: Year, Name, and Fastest Lap By (of the driver who performed the fastest lap of that race, indicated by their Twitter handle). You may remember that a Driver record can also be identified by this since the field has been defined as an External ID field:

| Year | Name | Fastest Lap By |
|---|---|---|
| 2014 | Melbourne | LewisHamilton |
| 2014 | Shanghai | LewisHamilton |
| 2014 | Monte Carlo | LewisHamilton |
| 2014 | Budapest | LewisHamilton |

Both the 2014 **Season** record and the `Lewis Hamilton Driver` record should already be present in your packaging org. If not, refer to the previous chapter to create these. Now, run the **Data Import Wizard** and complete the settings as shown in the following screenshot:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_128_0.png)

Next, complete the field mappings as shown in the following screenshot:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_128_1.png)

Click on **Start Import** and then on **OK** to review the results once the data import has completed. You should find that four new **Race** records have been created under `2014 Season`, with the **Fastest Lap By** field correctly associated with the `Lewis Hamilton Driver` record.
