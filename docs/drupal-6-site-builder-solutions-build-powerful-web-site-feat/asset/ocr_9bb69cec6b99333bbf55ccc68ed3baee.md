| Enabled | Name | Version | Description |
|---|---|---|---|
| ☐ | **Content** | 6.x-2.0-beta | Allows administrators to define new content types.<br>Required by: Content Copy (disabled), Content Permissions (disabled), Fieldgroup (disabled), Node Reference (disabled), Number (disabled), Option Widgets (disabled), Text (disabled), User Reference (disabled) |
| ☐ | **Content Copy** | 6.x-2.0-beta | Enables ability to import/export field definitions.<br>Depends on: Content (disabled) |
| ☐ | **Content Permissions** | 6.x-2.0-beta | Set field-level permissions for CCK fields.<br>Depends on: Content (disabled) |
| ☐ | **Fieldgroup** | 6.x-2.0-beta | Create field groups for CCK fields.<br>Depends on: Content (disabled) |
| ☐ | **Node Reference** | 6.x-2.0-beta | Defines a field type for referencing one node from another.<br>Depends on: Content (disabled), Text (disabled), Option Widgets (disabled) |
| ☐ | **Number** | 6.x-2.0-beta | Defines numeric field types.<br>Depends on: Content (disabled) |
| ☐ | **Option Widgets** | 6.x-2.0-beta | Defines selection, check box and radio button widgets for text and numeric fields.<br>Depends on: Content (disabled)<br>Required by: Node Reference (disabled), User Reference (disabled) |
| ☐ | **Text** | 6.x-2.0-beta | Defines simple text field types.<br>Depends on: Content (disabled)<br>Required by: Node Reference (disabled), User Reference (disabled) |
| ☐ | **User Reference** | 6.x-2.0-beta | Defines a field type for referencing a user from a node.<br>Depends on: Content (disabled), Text (disabled), Option Widgets (disabled) |

To use CCK, you will need to enable the Content module, which will allow you to create new content types and add custom fields to the new type. You will also need to enable the modules that provide the various field types. In the default installation, these are:

+   - **Text:** This allows free form text to be entered
+   - **Number:** This allows numeric values to be entered
+   - **Node Reference:** This allows links to other nodes (pages, content types, and so on) to be inserted in a type
+   - **User Reference:** This allows links to users who are registered on the web site

There are also several additional modules, which provide more content types that you can install. These can be found on the Drupal site at: `http://drupal.org/project/Modules/category/88`.

The following modules, which are part of the default CCK module, provide different methods of viewing, editing, and administering fields:

+   - **Option Widget:** This provides checkboxes and option buttons for various fields. These can be very helpful if your content fields need to be restricted to specific values.