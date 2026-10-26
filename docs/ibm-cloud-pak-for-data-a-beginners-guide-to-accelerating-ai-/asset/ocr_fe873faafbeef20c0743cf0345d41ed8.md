In this section, we discussed the semantics of permissions and how roles are defined or updated to suit the enterprise's requirements on access control and to identify suitable personas.

Roles are relevant to individual users as well as user groups, that is, roles can be assigned to Cloud Pak for Data User Groups, which implies that all members of that group will automatically inherit this role. In the following section, we will look more closely at how users are identified and groups are defined.

# User management and groups

Cloud Pak for Data exposes APIs and a user experience for Admins to be able to manage authentication and role-based authorization for each user. The following figure provides an example of the user experience that helps the Admin grant and revoke access to users.

![](bbox=[0.12, 0.385, 0.83, 0.615])

An admin (or a user with the manage users permission) can authorize additional users to access Cloud Pak for Data.

For example, via the user experience, a new user could be introduced and assigned appropriate roles: