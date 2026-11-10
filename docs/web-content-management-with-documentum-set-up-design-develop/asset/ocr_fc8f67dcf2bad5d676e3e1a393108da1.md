# 16.1 Anatomy of the FolderMap.xml File

Refer to figure 16.3 to understand the various XML elements in FolderMap.xml.

| Folder Map Element | Function Performed |
|---|---|
| `<rule>` | Denotes each rule specified in the Folder Map.
Each `<rule>` can have 0 or more `<attr_list>` tags and 1 or more `<path_list>` tags. |
| `<attr_list>` | Denotes a list of attribute rules/criteria. The Folder Map rule applies only when all the property value conditions specified in this tag are met. |
| `<attr>` | This is a name-value pair and denotes one property name and its value. |
| `<name>` | Name of the Docbase property, e.g. subject. |
| `<value>` | The value of the Docbase property. |
| `<path_list>` | Denotes a set of paths/locations where the content files will be placed if the rules match. |
| `<path>` | The full path within the Web Publisher Docbase where the content files will be placed. |

Figure 16.3: Folder Map XML elements

# 16.2 Limitations of Folder Map

Like all good systems, Folder Map has its share of limitations that we should be cognizant of while designing mapping rules for content.

Listed below are some of the limitations of Folder Map in Web Publisher. For an exhaustive coverage of Folder Mapping, please go through the Documentum Web Publisher Administrator guide.

+   - Web Publisher does not allow you to change the name of the Folder Map file (`FolderMap.xml`).
+   - The permission set (ACL) for `FolderMap.xml` is by default WebPublisher User Default ACL and cannot be changed.
+   - If the folder structure (specified in `FolderMap.xml`) does not exist in the Docbase, Folder Mapping automatically creates the folders in the Docbase. However, if the specified Web Cabinet does not exist in the Docbase, Folder Mapping does not automatically create it and throws an error instead.
+   - Folder mapping works from the topmost rule of the `FolderMap.xml` file to the last rule specified in the file. We need to define the specific rules first and then follow up with some generic rules. For those familiar with exception handling best practices in Java, this is analogous to catching the application-specific exceptions before handling generic exceptions.
+   - As soon as a particular rule is matched, Folder Mapping stops scanning the `FolderMap.xml` file further.

Enough of theory? Let us now understand how Folder Mapping works with the aid of a few practical examples.