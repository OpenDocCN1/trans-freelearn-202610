In both cases, if enforcement is enabled and user object or field level read permissions have not been granted for the object or fields referenced in the getSObjectType and getSObjectFieldSetList methods, an exception will be thrown.

# Overriding the default behavior

You may wish to disable the aforementioned default enforcement in cases where objects are read by the application code on behalf of the user. For example, reading the race data to update other areas of the application on behalf of the user. Such access is often referred to as system level access. In these cases, you may not wish to have this check enforced.

Allowing this enforcement check to execute in some cases may not be desirable since, as given in the preceding example, it then requires users to be granted access to the underlying race data object in order to use aspects of the application that read but do not display race data to the user. Another motivation for disabling the selector default behavior would be a preference to code these checks in your controller logic more explicitly.

Constructor parameters provided by the SObjectSelector class allow each Selector class to control whether security enforcement is enabled or not. This is also available to any code instantiating a Selector class, allowing this decision to be made on a per use case basis, for example, when the selector is used within a Controller class.

To make it easier to configure the fflib_SObjectSelector class throughout the application as well as to provide a means to establish a place for shared or common Selector logic code, a new base class can be created as follows:

```
public abstract class ApplicationSelector
extends fflib_SObjectSelector {

public ApplicationSelector() {
this(false);
}

public ApplicationSelector(Boolean includeFieldSetFields) {
// Disable the default base class read security checking
// in preference to explicit checking elsewhere
this(includeFieldSetFields, false, false);
}

public ApplicationSelector(
Boolean includeFieldSetFields,
Boolean enforceCRUD,
```