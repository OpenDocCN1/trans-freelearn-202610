The following table compares the Master-Detail Relationship and Lookup Relationship:

| Master-Detail Relationship | Lookup Relationship |
|---|---|
| You can define master-detail relationships between two custom objects, or between a custom object with a standard object (the standard object must be the parent). | You can define the relationship between any two objects, standard, or custom object. |
| When a record in the master object (parent) is deleted, all records in the detail object (child) related to the master record will be deleted. | You can configure the child object to control when the parent is being deleted, either to clear the parent record value in the child record, or to not allow deletion of the parent record. |
| All child records must have a related parent record. | The parent record may not be required, but you can configure to make the parent field required. |
| The ownership of a child record is determined by the related parent record; the child record does not have an owner. | Each child record has an owner. |
| A detail record inherits sharing and security from the master record. | There is no security sharing or inheritance between related parent and child records. |
| You can relate an existing custom object as a child object, but no records should exist in the child object. | To relate an object to another object, there is no validation on the existing number of records for the child object. |
| If you have a Roll-Up Summary field in the parent object, the create, edit, or delete actions in a child record will trigger edit action in the parent object. If you have validation rule in the parent object, it will trigger for the parent object as well. | You cannot create a Roll-Up Summary in Lookup Relationship using out-of-the-box Salesforce functionality. |
| Supports cross-object workflow; you can define to update a field in the parent record using the value from the child record. | Does not support cross-object workflow. |
| Ability to configure for a child record to allow re-parent to a new parent. | A child record can always re-parent to another parent record. |
| Ability to configure the sharing setting to allow to create, edit, or delete related Detail records based on permission on the parent record. | No sharing setting dependency between parent and child objects. |