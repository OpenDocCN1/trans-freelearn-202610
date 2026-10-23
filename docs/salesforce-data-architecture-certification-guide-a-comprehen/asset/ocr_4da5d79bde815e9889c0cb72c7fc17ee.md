If a territory is part of a Territory Hierarchy, then Territory records above an assigned territory in the Territory Hierarchy inherit the sharing of the assigned territory. Let’s put that as an example to explain it in more detail.

Let’s imagine that the Territory Hierarchy shown in the following diagram has been configured. Let’s assume that the OWD has been set to Private for Account and Opportunity records, and users assigned to the Territories don’t otherwise have access to the records without territory management in use. The sharing denoted in the following diagram will be the sharing as set for each territory in the Territory Hierarchy:

![](bbox=[0.09, 0.28, 0.85, 0.6])

Figure 12.1 – An example territory model

In our example, users assigned to the East Anglia territory record will have Read access to the Essex, Suffolk, and Norfolk Territory records. Despite the access on the East Anglia Territory being R/W (Read/Write), Territory Access isn’t inherited. Therefore, the access to the child Essex/Suffolk/Norfolk records will remain as Read-Only.

# Important note

Automatic territory assignment to Opportunity records requires editing a generated Apex class, which requires Apex coding skills. Apex coding is outside the scope of the exam, so it won’t be covered in this book.

Territory assignment for Account records can be configured to run automatically as part of the declarative functionality for Account Assignment rule creation from the Territory Model Hierarchy screen.