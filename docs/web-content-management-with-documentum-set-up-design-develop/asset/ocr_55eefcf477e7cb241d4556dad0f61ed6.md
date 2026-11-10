The following example of an ACL on a document belonging to the HR department in an organization explains the utility of this feature:

# HR_ACL

```
Group name: HR_Department
    Permit Type: AccessPermit
    Permission Level: Delete

Group name: HR_Department_subgroup
    Permit Type: AccessRestriction
    Permission Level: write
```

This ACL generically provides delete access to the HR department group, but does not allow a particular subgroup within this HR department to update or delete the document. By using AccessRestriction, the system allows this sub-group to only browse, read, relate (annotate), and version the document.

+   - **Flexibility to create groups as Dynamic Groups**: A Dynamic Group allows your application to find out whether the users specified in the group's membership list are considered members of this group or not by default when they establish a connection with the Docbase repository.
+     Say for example you assign a group abc as a Dynamic Group, setting its default membership behavior to treat its users as not members of this group. In the event of a user accessing the Docbase repository from a secure application, your application could add this user to the group but if the access is from a non-secure application, this user is not added to the group. Thus, using Dynamic Groups can assist you in setting up a group (or a role-based) security.
+   - Java programs have been supported in lifecycle Entry Criteria, Actions, Post Change, and for validation purposes.
+   - Improvements in workflows, such as support for work queues and a workflow timer for automatically resuming the suspended activities.
+   - Support for Global Login Tickets.
+   - Login Tickets are ASCII-encoded strings used by applications in place of a user's password while establishing a connection with the Docbase repository. A Global Login Ticket is an extension to this; it is a Login Ticket that can be accepted by any server of a trusted Docbase repository.
+   - The verity_locale key in server.ini file has become obsolete and the verity_location attribute in dm_server_config object type has been deprecated in the 5.3 release.
+   - Introduction of Privileged Groups in the system: Members of these system groups have special privileges for performing specific operations that they do not individually have.