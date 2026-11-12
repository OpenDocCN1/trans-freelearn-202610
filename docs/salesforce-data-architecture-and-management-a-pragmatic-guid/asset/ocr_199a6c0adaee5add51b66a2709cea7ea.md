- Sends an email and updates the record – This description is again generic and doesn't provide any specifics unless the workflow is explored further. It is very challenging to even get an idea of what it does.

+   • **Field update**: Since this is setting fields to new values, use the convention of *Set field name – new value of the field*. This allows the viewer to determine at a glance which field is being updated. Use whole words and, if required when space is limited, well-known abbreviations can be used. Always use the description field to provide details on what the field update does.
+   
+   **Do:**
+   - *Onboarding Completed – True* – Describes the field and then the new value that is being set.
+   - *Update the onboarding completed checkbox to true* – This example of the field update description provides sufficient details to the viewer at a glance.
+   
+   **Avoid:**
+   - *Stage Set to Booked* – In this example, this is describing the triggering event, and not what the field update is actually doing.
+   - *Stage is set to booked* – This description doesn't provide the details that the user can use to determine what this field update does and whether it is used anywhere else.

+   • **Approval process**: Similar to workflow rules, use the triggering event in the name rather than the actions. That way, you have flexibility in terms of updating the actions later on by adding further approval steps. Use the *Object – Triggering Event* convention for approval process names.
+   
+   **Do:** *Quote – Credit Terms Approval* – An example of an approval process name. Notice how it starts with the object that approval is on and the approval function. In this example, we are assuming that not all quotes need to be approved, but rather those that have credit terms requested on them.
+   
+   **Avoid:** *Approval and Rejection Email Process* – Contrary to our aforementioned example, this shows the actions the approval process will perform rather than providing an understanding of the intent of the approval process.

+   • **Approval step**: Use the convention similar to the approval process described previously, but in place of a triggering event, provide a description of what the approval step does.