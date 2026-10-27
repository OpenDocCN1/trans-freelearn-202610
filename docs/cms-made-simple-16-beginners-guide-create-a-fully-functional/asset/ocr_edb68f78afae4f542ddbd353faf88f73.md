In the admin console (Site Admin | Global Settings), under the Advanced Setup tab, you can define Basic Properties for the additional editors such as Image. We used this field in the last chapter. Here, the editor can choose an image from a drop-down list and thus assign an image to a certain location in the template defined by the designer.

![](bbox=[0.205, 0.285, 0.27, 0.345])

There is a useful third-party module called Template Externalizer. It allows you to use any external editor to edit your templates and stylesheets. After installing and turning on Development Mode, the module automatically extracts all stylesheets and templates into the specified directory.
If you modify templates in this external directory, changes are automatically detected, imported to the database, and take effect immediately. When the module is installed, there is the permission Template Externalizer that can be granted to the group Designer.

# Viewing the admin log

The admin log records the details of the changes made to the site by all users. This log can be found in the admin console (Site Admin | Admin Log) and includes records about the creating, deleting, and changing of:

+   - Pages and global content blocks
+   - Templates and stylesheets
+   - Users, groups, and permissions
+   - Site preferences
+   - Modules

At the top of the log, you will find a navigation section providing links to the first, next, previous, and last pages of the log. There is also a link to download the complete log as a tab separated text file. At the bottom is the Clear Admin Log link. This will clear the entire log.

![](bbox=[0.205, 0.705, 0.27, 0.745])

Important! You won't be asked to confirm the Clear Admin Log, and this action cannot be undone once completed.

Once the admin log is cleared, there is no evidence of who has changed anything on the website and what the changes were. Therefore, there is even a separate permission called Clear Admin Log that is granted separately from the permission Modify Site Preferences.