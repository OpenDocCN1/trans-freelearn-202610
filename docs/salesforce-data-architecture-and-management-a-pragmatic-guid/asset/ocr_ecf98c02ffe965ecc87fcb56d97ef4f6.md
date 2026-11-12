Referential integrity
Referential integrity refers to the consistency of data within a relationship, for example, an invoice line-item object would have a master-child relationship with the invoice object being the master. Invoice line items each refer to the invoice record 12345. If the record 12345 was deleted, the five invoice line items would become orphaned. Referential integrity prevents these types of scenarios from occurring.

+   Lookup relationships: These are very similar to master-detail relationships in that if the record being looked up to (the target) is attempted to be locked, a lock contention may arise causing the insertion/update to fail. Use the same aforementioned techniques described for master-detail to avoid lock contention.
+   
+   When a lookup is defined, you have the option to select either one of the following:
+   i) Clear the value of this field. You can’t choose this option if you make this field required
+   
+   ii) Don’t allow deletion of the lookup record that’s part of a lookup relationship
+   
+   This may seem trivial but during large data loads, what you have selected on your lookup definition may cause loads to fail. This is because Salesforce treats the two differently. When the former is selected, that is, Clear the value of this field. You can’t choose this option if you make this field required, Salesforce just ensures that the lookup value on the target object exists. When the latter is selected, it locks the target lookup record, possibly leading to a lock contention situation.

+   Triggers: When loading records that fire a trigger, the trigger logic may cause the system to attempt a lock on a record other than the trigger-originating record and cause lock contention. To avoid lock contention, disable triggers or have logic in your trigger code to avoid such situations.

+   Workflow rules: Salesforce locks the records when a workflow rule triggers a field update and if multiple threads are attempting to lock the same record, lock exceptions can happen; for example, a workflow rule that’s attempting to update the same parent record in multiple threads will cause lock contention. To avoid these locks, disable the workflows before loading.