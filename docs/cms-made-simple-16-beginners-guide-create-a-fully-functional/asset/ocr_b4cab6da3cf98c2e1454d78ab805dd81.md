# Users and Permissions

| Permission | Suggestion for the role | Description |
|---|---|---|
| **Modify Users** | Admin | Allows editing the user with the exception of the administrator user account. |
| **Remove Global Content Blocks** | Admin | Allows editing the global content blocks. Depends upon the permission **Modify Global Content Blocks**. |
| **Remove Groups** | Admin | Allows deleting user groups. |
| **Remove Pages** | Editor | Allows deleting content pages where the user is an **Owner** or **Additional editor**. |
| **Remove Stylesheet Associations** | Designer | Allows removing stylesheet associations. |
| **Remove Stylesheets** | Designer | Allows removing stylesheets. |
| **Remove Templates** | Designer | Allows removing layout templates, but not the module templates. |
| **Remove Users** | Admin | Allows removing users with exception of the own account and the administrator account. |
| **Advanced usage of the File Manager module** | Admin | Allows viewing the complete structure of the website and not only the `/uploads` directory. For this permission to work, the advanced mode setting should be enabled in **File**. |
| **View Tag Help** | Editor | Allows reading help for the custom smarty plugins. |

# Module permissions

Not only can the core functions and core modules of CMS Made Simple have permissions, but every third-party module you have installed on your page can add the module's specific permissions to the list. If you still have the **Questions & Answers** module installed, as described in the last chapter, you will find the permissions **Manage Questions**, **Can Answer Questions**, and **Can Approve Questions** in your list of permissions (**Users & Groups | Group Permissions**).

The **Archiver** module has the permission **Manage Archives** that can be granted to advanced users with access to the admin console or to the main editor with the permission **Manage All Content**.