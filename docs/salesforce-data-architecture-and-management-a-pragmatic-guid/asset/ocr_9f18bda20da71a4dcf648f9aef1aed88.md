Unless you are absolutely sure they are needed, try not to have the required fields at the object level. The reason is that any integrations that you put in will now need to populate the required fields as well. This may not work well for scenarios where the required field must be populated when a user is doing data entry but is not required when data is brought in via integration from another system.

# Formulas and roll-up summary fields

This is another one where you don't want to be excessive with it, but also too few of these fields indicate that things may be being done in code. During assessments, I will look for key objects, such as `Account`, `Opportunity`, `Quote`, and, if it's a custom application, objects that are on the parent side in a parent-child relationship, as well as roll-up summary fields.

> **Limits**
> 
> There is a limit on the number of parent-child or master-detail fields you can have on an object. There is a performance cost associated with these fields, and therefore, you want to be careful and use these only when it's absolutely needed. There are similar limits on roll-up summary fields, which out of the box only work with master-detail fields.

Similarly, look out for formulas on objects that are critical to the application and review the code to check whether there is logic in the code that could have been more suited to formulas.

# Record types

Record types are one of the many powerful features that the platform provides, but being excessive may be detrimental. This is because record types control picklist values and, depending on how many picklist fields you have, it can quickly become cumbersome to maintain these for too many record types in particular. I recommend having no more than 5–7 record types per object. Having excessive record types in the org usually means there are as many page layouts for the object, which can be an administrative overhead to maintain.

# Profiles

At the outset, the best practice is to control access via permission sets. It may be very tempting to create profiles to control access, but a large number of profiles can become an admin's nightmare. Consider an organization where you have 15 profiles and 100 fields on 10 objects. The initial setup alone for read-only access can involve a review of 15,000 checkboxes and another 15,000 for editing.