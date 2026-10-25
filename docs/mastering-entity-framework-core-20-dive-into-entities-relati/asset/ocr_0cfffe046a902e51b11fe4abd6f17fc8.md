# 6
Save Yourself – Hack Proof Your Entities

In Chapter 5, Know the Validation – Explore Inbuilt Validations, we understood about in-built validations available in the Entity Framework Core. We explored each one of the validation in detail by starting with how the validation were handled earlier, the way how framework handles it, the configurations required and the rendering performed by the framework.

In this chapter, we will address the following concerns with respect to data security:

+   - What should be done to the data before we send it over the network?
+     - Could you make a wild guess on what should be performed before we send the data over the network?
+   - What if someone hacks the client-side validation? Or the scripting stops working?
+     - We could shield ourselves by introducing server-side validation.
+   - Beyond client-side and server-side validations, do we need anything in addition?
+     - Yes, usual server-side validations would look for any errors and validators of all properties of the model would be taken into account by manual validations.
+   - Hoping we have custom implementations of validations as well?
+     - Yeah, we do have provision to create custom validators and .NET Core's data-attribute approach for custom validators as well.
+   - Damn, we should have covered almost everything. Guess we didn't leave behind anything else?
+     - Except for one thing—remote validation. For instance, checking username availability could be done using remote validation from the client-side without adding any client-side code.