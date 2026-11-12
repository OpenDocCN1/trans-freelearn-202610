Following the same technique and order you used before, you can think of some potential solutions:

+   Use custom validation rules: They can be configured to trigger upon attempting to save an edited record. However, they do not prevent the user from attempting to edit the record. They will fire only upon attempting to save the record. If you are going to use validation rules, you need to come up with an efficient way to prevent editing the record's fields. You cannot simply list every single field in your validation rule.

Introducing a new custom is_locked__c field and updating it using a flow upon closing the opportunity could be a solution. Your validation rule would then rely on this field alone. However, if you are after a good user experience (and you should be), you need to keep in mind that this solution alone would not prevent the user from attempting to edit the record.

+   Update the record type upon closing the opportunity: A flow can be used for this job too. You can then assign a page layout with read-only fields to that record type. Some fields cannot be set to read-only, such as name, currency, closed date, stage, and some others. This solution also does not prevent the record from being edited via code or an API (for example, via a data loader). You should consider this solution as complementary to other approaches but not as a comprehensive solution on its own.

+   Use Apex validations in a before-update trigger: This gives you the ultimate capability and flexibility, but with additional development efforts. This approach will also cause a loss of agility as any future changes would require code edits. You should consider this option only for sophisticated use cases.

+   Lock the record via an approval process: You can lock the record completely using an approval process. However, note that only the administrator or users with Modify All access can edit the record in this case, which does not meet the requirement.

The requirement here can be completely fulfilled using configurations. There is no justification for using an Apex validation. You can propose a solution based on a combination of the first two approaches. Before drafting the end-to-end solution to present, have a look at the next three requirements as they are related to this.

> Opportunity line items' fields should also get locked. No products can be added, edited, or deleted once the opportunity is closed.

You understand that the lock should also be extended to the opportunity line item. You can introduce validation rules on the OpportunityLineItem object as well, which relies on its parent opportunity's is_locked__c flag. You might want to consider updating the record type as well. The requirement can still be fulfilled using configurable features. Now, explore the next requirement.

> The opportunity owner is allowed to change the value of the description field for up to seven days after closing the opportunity.