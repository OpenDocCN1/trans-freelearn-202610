| Execution context | Security | State management |
|---|---|---|
| Apex Controller Remote and Aura Actions

**Note:** Remote Actions methods are used by Visualforce and Aura Actions methods are used by Lightning Components | Sharing is not enforced by default; the developer needs to apply the `with sharing` keyword to the enclosing class. It is required for Salesforce security review.

CRUD and FLS are not enforced by default; the developer needs to check this in code. | Remote Action and Aura Action Apex methods are already static, as stated previously; any static data is not retained in Apex. Thus, there is no state retained between remote action calls made by the client JavaScript. Typically, in these cases, the state of the task the user is performing is maintained on the page itself via the client-side state. As a result of not having to manage the state for the developer, these type of methods are much faster than Action Methods described in the preceding section. |
| Apex Trigger | Sharing is not enforced by default; the developer needs to apply the `with sharing` keyword in the outermost Apex class (if present, for example, Apex Controller).

Triggers invoked through the Salesforce UIs or APIs run without sharing implicitly; if you need to apply with sharing, move your trigger code into an Apex class and call that from the trigger code.

CRUD and FLS are not enforced by default; the developer needs to enforce this in the code. | Static variables are shared between Apex Trigger invocations within an execution context. This allows some caching of commonly used information and/or recursive trigger protection to be implemented if needed. |