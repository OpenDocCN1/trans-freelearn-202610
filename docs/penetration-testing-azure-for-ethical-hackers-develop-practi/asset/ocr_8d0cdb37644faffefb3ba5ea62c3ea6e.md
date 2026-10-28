# Enabling a disabled user identity and resetting their password

A less suspicious method to establish a backdoor identity for an Azure AD tenant is to look for existing identities that are disabled, enable them, and reset their passwords. To enable disabled user accounts, the permission highlighted in the following table is needed. The built-in roles listed in the **Azure AD Role** column have this permission. From a pentesting perspective, we need to be cautious as an identity could be disabled because it has been compromised previously:

| Method | Permission Needed | Azure AD Role |
|---|---|---|
| Enable disabled user identity | `microsoft.directory/users/enable` | • User Administrator<br>• Global Administrator<br>• Custom roles cannot currently be used |

Table 8.3 – Roles for enabling disabled user identities

After enabling a user identity, password reset permissions in Azure AD depend on the role of the user that is being reset. These permissions are separated between administrator roles (global admin, authentication admin, and so on) and non-administrator user types.

Here are the built-in roles that can reset user passwords in Azure AD listed in ascending order of privilege:

+   - **Password Administrator:** Can reset passwords for non-administrators and other password administrators
+   - **Helpdesk Administrator:** Can reset passwords for non-administrators, password administrators, and other helpdesk administrators
+   - **Authentication Administrator:** Can reset passwords for non-administrators, password administrators, and other authentication administrators
+   - **User Administrator:** Can reset passwords for non-administrators, groups administrators, helpdesk administrators, password administrators, and other user administrators
+   - **Privileged Authentication Administrator:** Can reset passwords for all non-administrators and administrators
+   - **Global Administrator:** Can reset passwords for all non-administrators and administrators