+   - Typically you don't need to catch exceptions from the Service layer (unlike Visualforce controller action method callers), as the Apex REST runtime catches the exception and the returns appropriate REST API error code and message. Avoid implementing your own REST API error handling.
+   - The `global` modifier is still required. Keep in mind that the same packaging restrictions regarding changes to these methods apply.
+   - It is possible for the Developer X Apex developer to call these methods from Apex, as they are `global`. However, this should be discouraged because these methods might reference the REST API context variables, which will not be initialized in a standard Apex context.
+   - The `Set` Apex data type is not supported by the Apex REST methods. Unlike the standard, which has been established with the Service class methods, the Set Apex data type is not supported by the Apex REST methods. Therefore, you will need to convert the `List<Id>` REST method parameters to the `Set<Id>` collections for your Service method calls.
+   - The `ComplianceService.verify` method throws an exception with additional information about the specific compliance failures. However, information in a custom Apex exception is not automatically serialized by the platform — only the error message is included. In this case, a custom response would be required to catch the exception and return the information.

# # Calling your Apex REST application APIs

The Salesforce Developer Workbench can be used to call the preceding REST API. Locate a Driver record without the FIA Super License field selected and use the following ID. Note that the URI shown in the following screenshot contains the package namespace, fforce. To try this out you need to replace this with your chosen package namespace:

![](bbox=[0.24, 0.645, 0.775, 0.895])