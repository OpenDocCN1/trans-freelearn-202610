# What does the input mask character # for attributes signify?

A hash masking character (#) allows only numeric values (0 through 9) for the attribute in question.

# What is meant by Value Assistance?

Value Assistance is used for displaying a pre-defined list of values for a specific attribute. Users can choose from the set of values displayed in Value Assistance for the attribute. Note that this feature is not available for attributes of Boolean data type.

# With reference to Registered Tables, what is the alias used for referring to the Docbase owner of the RDBMS table?

dm_dbo is the alias used for referring to the Docbase owner.

# What are the various scopes that Documentum uses for resolving an alias?

The various scopes used by Documentum for resolving an alias are: lifecycle scope, workflow scope, session scope, user scope, and system scope.

# In the context of permission sets, what is the reserved alias that Documentum uses for referring to the current owner of the object and all the users present in Docbase?

dm_owner is an alias for the current owner of the object and dm_world is an alias for all Docbase users.

# What are the different states of a lifecycle policy object before it can be used by the applications?

The various states of a lifecycle policy object are: Draft (when the lifecycle has been created or modified but not validated), Validated (when the lifecycle has passed all system validation tests), Uninstalled, and Installed (when it has been validated and is ready to be used by applications).

# What are the two types of states that can be created in a Lifecycle State Diagram?

Normal and Exception states. Normal states are for the normal linear flow of object, while Exception states represent an abnormal flow or diversions in the linear flow.

# Mention a few standard Actions that can be performed by the system in a lifecycle state.

A few standard Actions that can be performed by the system in a lifecycle state are: adding or removing version label, setting owner, setting permission set, requesting rendition, etc.

# Can one specify Java classes as procedures in lifecycle Entry Criteria, Actions, PostChange, and for validations?

Yes, this is possible in Documentum release 5.3.

# Mention a few mechanisms to promote an object through the various states of a lifecycle.

A few mechanisms to promote objects through lifecycle states are: Web Publisher Promote menu option, workflows, promote server API command, and the BATCH_PROMOTE administration method.