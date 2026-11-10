# Documentum 5.3 Update

Documentum release 5.3 has introduced a new feature called **Modules** in Documentum Application Builder. Modules are nothing but elements of executable code that are represented in the Docbase repository as a `dmc_module` folder object type. For example, Business Objects in Documentum can be treated as a kind of module. This allows you to package your business object classes and supporting files (in the form of JAR files) in a DocApp that can be installed via Documentum Application Installer on a target environment.

2. After you have included the objects in the DocApp, select individual objects in the left-hand pane and choose menu option DocApp | Set Installation Options to set their installation options.

Setting installation options is extremely important because by doing so you provide a lot of vital instructions to the Application Installer for installing objects in the target Docbase such as:

+   - **Upgrade options**: If DocApp Installer finds an object with the same name as in your source Docbase, it can be directed to perform one of the following options:
+     - Overwrite the corresponding object in the target Docbase with the version existing in the source Docbase.
+     - Update and create a new version of the object in the target Docbase, leaving the original version(s) intact.
+     - Do not overwrite the object in the target Docbase.
+   - **Data object transfer**: This option is available in the case of XML Applications, Folders, and Cabinets and it directs the installer to include or exclude objects in the manner described as follows:
+     - **All content**: Installer will install the specified cabinets, folders, and sub-folders and all objects contained within them.
+     - **Top level content**: Installer will install the specified XML Application folder or folder and the objects contained within it.
+     - **Hierarchical structure**: Installer will install only the specified cabinets, folders, and sub-folders but no objects.
+     - **Object only**: Installer will install only the specified XML Application folder, cabinet, or folder.

Let us take an example of custom lifecycle object `Custom_Lifecycle` and set its installation options.