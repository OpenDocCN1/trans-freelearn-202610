# User and group access

An ACE may specify a particular user or it may specify a directory group. In the latter case, the ACE applies to all users who are directly or indirectly members of that group. (Indirect membership occurs because some directories allow groups to be nested within other groups.)

You could theoretically use only user ACEs and never use group ACEs, but that's usually a bad idea. In the first place, it would be pretty tedious and labor-intensive to list individual users when a group would do the job. In the second place, a group ACE is used dynamically in the sense that the group membership is evaluated at the time the ACE is examined (modulated slightly by the short-term caching in the CE). So, if a new user is added to a group, the ACE is automatically applicable to that user. If a user is removed from a group, the rights conferred by that group ACE are automatically withdrawn.

It's a best practice to use group ACEs wherever possible. The ACL is an attribute of an individual object, and it is generally impractical to update the security on thousands or even millions of objects as individual users come and go.

> There is another kind of ACE that is of interest only to application developers. It is an unknown ACE. It comes about because ACEs sometimes get orphaned. The user or group mentioned in the ACE gets deleted from the directory, but the ACE still exists in the repository. These ACEs will never match any calling user and so will never figure into any access control calculation.
> 
> Application developers have to be aware of this kind of ACE when programmatically displaying or modifying the ACL. The unknown ACEs should be silently filtered out and not displayed to end users. (FEM displays unknown ACEs, but it is an administrator tool.) If updates are made to the ACL, the unknown ACEs definitely must be filtered out. Otherwise, the CE will throw an exception because it cannot resolve the user or group in the directory.

# Rights

Every action that a user can take that is controlled by an access check has an individually named access right in the CE. At the end of an ACL evaluation, a given access right is either allowed or denied (perhaps via implicit denial) to the calling user. There is never any shading or doubt: the user is allowed or denied for a given right. Likewise, mention of a right in an ACE is always an absolute "allow" or "deny" within the scope of that ACE. Rights are considered individually, and being granted or denied one right does not automatically grant or deny a user any other rights.