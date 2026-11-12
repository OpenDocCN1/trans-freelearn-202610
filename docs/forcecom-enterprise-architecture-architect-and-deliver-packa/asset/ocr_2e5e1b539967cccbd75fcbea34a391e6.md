The documentation states that when using components such as `apex:inputText` and `apex:outputText`, these will not enforce field security. While this is correct, security is always applied to SObject field expressions as well. So, if a field is not visible (as opposed to just read only), even the `apex:outputText` component will not display a value. However, if a field is read only and is used with `apex:inputText` (for example), it will render as an editable field. Thus, the general guideline here is that, if you have an SObject field expression, use the recommended `apex:inputField` and `apex:outputField` components.

A common confusion point is the use of the `with sharing` and `without sharing` keywords on the associated controllers. These keywords have no effect at all on the enforcement of field-level security; they purely determine record visibility, and thus effect only SOQL queries made within the controller and other Apex classes that make SOQL queries that are called by this controller.

In addition to using Apex described within your controller code, you can also determine directly a field's accessibility, using the `$ObjectType` global variable. For example, you can use the following expression to hide an entire section based on a given field's security. `{!$ObjectType.Race__c.fields.Status__c.Accessible}`.

The following Visualforce page and associated custom controller illustrates field-level security. In the use case shown here, two users are used, one has full access and the other has been given the following permissions via their profile:

+   * Read-only access to the `Status__c` field
+   * No access at all to the `FastestLapBy__c` field

First, review the Visualforce page; each section shows the following use case:

+   * Use Case A shows the use of `apex:inputField`
+   * Use Case B shows the use of `apex:outputField`
+   * Use Case C shows the use of `apex:inputText`
+   * Use Case D shows the use of `apex:outputText`
+   * Use Case E shows the use of an SObject field expression
+   * Use Case F shows the use of a controller property expression