Thus, the usage code now looks like this:

```
List<Race__c> races =
    new RacesSelector().selectById(raceIds);
```

# Standard features of the Selector base class

The fflib_SObjectSelector base class contains additional functionality to provide more query consistency and integration with the platform. These apply to the aforementioned selectSObjectsById method as well as your own. The following sections highlight each of these features.

> You can, of course, extend the standard features of this base class further. Perhaps there is something you want all your queries to consider, a common field or aspect to your schema design, or a feature that is common to most of your selectors. In this case, create your own base class and have your selectors extend that, as follows.

```
public abstract class ApplicationSelector
    extends fflib_SObjectSelector
{
    // Add your common methods here
}
public class RacesSelector
    extends ApplicationSelector
{
    // Methods using methods from both base classes
}
```

# Enforcing object and field security

Salesforce requires developers to implement the object read security and field level read security checks when querying. Typically using the methods on the SObjectDescribe and SObjectFieldDescribe Apex runtime types. The following sections describe the fflib_SObjectSelector base class support in respect to this.

# Default behavior

By default, the base class methods in the fflib_SObjectSelector base class automatically perform Object read security. The ability of the base class to enforce Field level read security is also available but is not enforced by default.